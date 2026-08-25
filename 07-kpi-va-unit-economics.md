# 07 — KPI, tracking và unit economics

## 1. Nguyên tắc đọc số liệu

1. Mỗi KPI phải có **câu hỏi, grain, tử số, mẫu số, nguồn, cửa sổ thời gian, maturity rule, formula version và owner**.
2. Tách calendar activity khỏi cohort funnel. Không lấy order tháng này chia click tháng này nếu attribution/reporting kéo dài sang tháng sau.
3. Không ép Search Console, analytics, event log, router và affiliate network bằng nhau; chúng đo các việc khác nhau.
4. `Pending/Ordered` chưa chắc chắn; `Final` chưa phải tiền mặt; `Paid/received` mới là tiền thực nhận.
5. Khi mẫu hoặc maturity chưa đủ, trạng thái là `LEARNING / NOT_ENOUGH_DATA / NOT_APPLICABLE`, không phải zero hay thất bại.
6. Disclosure, data accuracy, privacy, link safety và reconciliation đứng trước CTR/doanh thu.
7. Dashboard lưu raw operands; rate phải được tính lại từ operands bằng `kpi_definition_version`, không gõ tay để “khớp”.

## 2. Chuỗi đo lường và nguồn sự thật

```text
Google impression/click                    ← Search Console
Qualified session/utility/result           ← Web analytics + Event/Click Log
Product outbound click                     ← Event/Click Log
Optional router click                      ← Router/server log
Network accepted click/order               ← Affiliate dashboard/API/export
Valid/final/reversed commission             ← Affiliate dashboard/API/export
Payout statement                            ← Affiliate payout statement
Net cash received                           ← Bank/payment evidence
Cost/time/allocation                        ← Cost Log
```

### Không được suy diễn

- `product_outbound_click` không chứng minh network đã nhận click.
- Network click không chứng minh order.
- Order reported không chứng minh valid/final.
- Final commission không chứng minh đã nhận tiền.
- Statement sent không chứng minh net cash đã vào tài khoản.
- Search impression không phải website session.
- Sitemap submitted không bảo đảm crawl/index/rank.

### Direct link, optional router và soft launch

- `DIRECT`: anchor đi thẳng tới official affiliate/non-affiliate URL. Browser event là best-effort; không giữ navigation để chờ analytics.
- `ROUTER`: `/go/{link_id}` lookup active `link_version` và chỉ dùng khi program cho phép. Server có thể tạo và ghi `outbound_click_id` trước redirect.
- Mọi product link phát **một** custom event `product_outbound_click`.
- `affiliate_click` là event/metric dẫn xuất khi `link_type = AFFILIATE` và `link_id/link_version` active, bất kể `delivery_mode`; không phát thêm event browser thứ hai.
- Soft launch chỉ dùng non-affiliate link đặt revenue reconciliation là `NOT_APPLICABLE_NOT_MONETIZED` kèm test plan. Không điền order/final/paid bằng 0 để giả pipeline monetization đã được thử.

## 3. Event contract và ID

### ID namespace

- `JNY-`: journey giả danh nối utility → result → click.
- `REC-`: recommendation run.
- `EVT-`: event.
- `CLK-`: outbound click.
- `LNK-`: stable link; `link_version` xác định destination/rule bất biến của lần phát hành.
- `OFF-`: commercial offer, tách khỏi link/version.
- `JOB-`: scheduled/manual bot job; không dùng chung với recommendation run.
- `IMP-`, `ORD-`, `COM-`, `PAY-`: import, order, commission, payout business IDs.
- `OEV-`, `CTR-`, `PAL-`: order observation, commission transition và payout allocation append-only.

`journey_id` và `pseudonymous_session_id` không chứa direct identifier/Google-prohibited PII, nhưng vẫn có thể là personal data theo luật áp dụng. Một journey có thể có nhiều recommendation runs và nhiều outbound clicks.

### Events tối thiểu

Mọi event có common envelope: `event_id`, `event_version`, `occurred_at`, `journey_id`, `pseudonymous_session_id`, `page_id/page_version`, `consent_state`, `is_internal_or_test` và dedupe key theo Event Contract. Bảng dưới chỉ liệt kê thêm các parameter đặc thù:

| Event | Trigger duy nhất | Params tối thiểu | Không gửi |
|---|---|---|---|
| `utility_start` | Tương tác đầu tiên đủ điều kiện với utility trong session | interaction type | Free text, email |
| `utility_complete` | Engine nhận input hợp lệ và hoàn tất response | recommendation_run_id, no_result, eligible_count, data/scoring version | Profile có thể nhận diện |
| `result_view` | Kết quả thực sự hiển thị theo rule đã định | journey_id, recommendation_run_id, result_type, product_count | Affiliate URL/token |
| `comparison_open` | Người dùng mở compare | journey_id, recommendation_run_id, product IDs | Tên/email |
| `product_outbound_click` | Người thật bấm product CTA | event_id, journey_id, recommendation_run_id, product_id, link_id/version, offer_id/version nếu có, link_type, delivery_mode, placement | PII, full secret URL |
| `feedback_submit` | Feedback hợp lệ được gửi | journey_id, recommendation_run_id, useful flag/reason code | Free text khi chưa có privacy design |
| `data_error_shown` | UI hiển thị data/no-safe-result error | journey_id, recommendation_run_id, error code | Stack trace/secret |

Dùng [Event Contract](templates/21-event-contract.csv) để khóa trigger/grain/dedupe và [Event/Click Log](templates/27-event-click-log.csv) cho record append-only. Google Analytics có thể dùng recommended/custom events và DebugView; xem [GA4 events](https://developers.google.com/analytics/devguides/collection/ga4/events) và [outbound clicks](https://support.google.com/analytics/answer/13566436).

## 4. Grain, mẫu số và maturity

### Qualified Session

MVP dùng **session**, không dùng lẫn “visitor/session”:

- Relevant page/utility load thành công.
- Không phải known bot, health check, internal/test traffic.
- Đáp ứng interaction rule được khóa trong Measurement Plan.

Tên field là `qualified_sessions`. Nếu sau này cần unique visitor, tạo KPI riêng với ID, consent, persistence và dedupe window riêng.

### Các grain khác

- `Result-viewing session`: session có ít nhất một result thực sự hiển thị.
- `Unique outbound-click session`: session có ít nhất một `product_outbound_click`.
- `Outbound click`: một `outbound_click_id` hợp lệ; một session có thể có nhiều click. Browser/server records trùng ID chỉ tính một logical click.
- `Order line`: grain chuẩn cho valid/rejected/final rate nếu network có line items.
- `Commission line`: không dùng thay order line trong một tỷ lệ order-grain.
- `Resolved order lines = valid + rejected`; pending không vào mẫu số.

### Maturity theo program

- `Attribution maturity`: click cohort đã qua attribution window + reporting delay.
- `Validation maturity`: order line đã qua validation/return window dùng để đọc valid/rejected ổn định.
- `Finalization maturity`: valid commission/order line đã đến thời điểm có thể final.
- `Payout due cutoff`: final commission đã đến due date; grace period được lưu riêng.

Mỗi program có mapping/rules trong [Network Status Mapping](templates/28-network-status-mapping.csv). Không dùng một `data_maturity` chung cho mọi bước hoặc trộn programs có window khác nhau mà chưa tính per-program.

## 5. KPI funnel đúng grain

| KPI | Công thức |
|---|---|
| Search CTR | Search clicks / Search impressions |
| Utility Start Rate | Unique utility-start sessions / Qualified sessions |
| Utility Completion Rate | Unique sessions có ≥1 completed recommendation run / Unique utility-start sessions |
| No-result Rate | No-result completed runs / Completed runs |
| Result-to-Outbound-Click Rate | Unique outbound-click sessions / Result-viewing sessions |
| Affiliate Clicks | Count distinct `outbound_click_id` của `product_outbound_click` với `link_type=AFFILIATE` và active `link_id/link_version` |
| Router-to-Network Acceptance | Matured network accepted clicks / Eligible router affiliate clicks cùng cohort |
| Click Conversion Rate | Distinct matured accepted click IDs có ≥1 attributed order / Matured accepted click IDs |
| Orders per Matured Click | Attributed order lines / Matured accepted click IDs |
| Order-to-Valid Rate | Valid order lines / Resolved order lines |
| Rejection Rate | Rejected order lines / Resolved order lines |
| Valid-to-Final Rate | Finalized eligible order lines / Eligible valid order lines |
| Commission Retention | Net Final Commission / Valid provisional commission amount của cùng eligible cohort |
| Attribution Match Rate | Exact click-ID matched conversions / Imported conversions |
| Unmatched Conversion Rate | Unmatched conversions / Imported conversions |

Recurring commission lines phải tách khỏi initial-order conversion rate bằng `conversion_type/commission_type`; các rate click→order của row recurring là `NOT_APPLICABLE` nếu không có order/click grain tương ứng. Khi network không hỗ trợ sub-ID/clickref, ghi limitation; match theo ngày/product không được gọi là `EXACT`.

Mọi order/commission amount trên dashboard đến từ **current-as-of view**, không phải tổng các dòng transition. Với `SNAPSHOT`, chọn observation/transition hiệu lực mới nhất không bị supersede; với `DELTA`, cộng signed deltas theo rule version. Loại internal/test, duplicate và quarantined batches trước khi tính. `Net Final Commission` là final hiện hành sau reversal/clawback/adjustment; không trừ reversal lần thứ hai.

## 6. KPI tiền, payout và cost

### Bốn trạng thái tiền

```text
Ordered Commission
Valid Provisional Commission
Final Commission
Paid Cash
```

### Final economics theo cohort

| KPI | Công thức |
|---|---|
| Final EPC — router | Net Final Commission / Eligible matured router affiliate clicks |
| Final EPC — network | Net Final Commission / Matured network accepted clicks |
| Final RPV | Net Final Commission / Qualified sessions của source cohort |
| Economic Contribution | Net Final Commission − allocated economic cash cost − imputed labor cost |
| Economic Profit/1.000 Qualified Sessions | Economic contribution / Qualified sessions × 1.000 |

### Payout/cash theo allocation

| KPI | Công thức |
|---|---|
| Gross Payout Coverage | Allocated gross payout / Final gross commission due |
| Net Cash Realization | Net cash received / Expected net after documented fees |
| Paid RPV | Net paid cash allocated về cohort / Qualified sessions của cohort đó |
| Cash Contribution | Net paid cash − allocated non-embedded cash outflow costs |
| Cash Profit/1.000 Qualified Sessions | Cash contribution / Qualified sessions × 1.000 |

Không chia statement net cho final gross rồi gọi là payout coverage. Nếu payout chưa allocation được về commission/click cohort, Paid RPV và Cash Profit/1.000 là `UNKNOWN`.

### Reconciliation materiality

```text
Open exception khi:
abs(expected - observed)
> max(materiality_absolute_by_currency,
      expected × materiality_percent)
```

Trong Payout Snapshot:

```text
gross difference = abs(final gross due - allocated gross)
net difference   = abs(expected net after known fees - received net)
```

Áp dụng threshold riêng cho từng difference; không gộp hai basis. Nếu expected bằng 0, difference rate là `NOT_APPLICABLE` và mọi amount không giải thích được mở exception. Một payout chỉ overdue khi `as_of_at > payout_due_at + grace_days`; `overdue_days` bắt đầu từ mốc đã cộng grace, không từ due date gốc.

### Chi phí

Theo dõi domain/hosting/database/analytics/monitoring, API/LLM/crawl hợp pháp, subscriptions, payment/FX fees, incident time và giờ research/fact-check/operations. Mỗi cost có scope, period, allocation basis và evidence trong [Cost Log](templates/19-cost-log.csv). Một `cost_entry_id` có thể có nhiều `cost_allocation_id`; tổng fraction của cùng allocation set phải bằng 1. Dashboard cộng allocated amounts, không cộng lại original amount lặp theo các lát phân bổ.

`cash_treatment` tách `SEPARATE_CASH_OUTFLOW` khỏi `EMBEDDED_IN_RECEIVED_NET`. Economic view tính chi phí cash liên quan trên basis final; cash contribution chỉ trừ outflow chưa nằm trong net cash. Ví dụ payment fee đã bị network trừ trước khi trả nằm trong `received_net_amount`: giữ nó để phân tích economics nhưng không trừ lại khỏi cash contribution. Setup cost và recurring cost tách; setup chỉ amortize khi policy viết trước.

## 7. Expected value và break-even

### Expected Net Final Commission

```text
Expected Net Final Commission Amount
= Matured Accepted Clicks
× Attributed Order Lines per Matured Click
× Order-to-Valid Rate
× Valid-to-Final Rate
× Average Net Final Commission per Finalized Eligible Order Line
```

Không thay `Attributed Order Lines per Matured Click` bằng distinct-click conversion rate nếu một click có thể tạo nhiều order lines; nếu buộc dùng click conversion thì phải nhân thêm average attributed order lines per converted click. Các tỷ lệ và average phải cùng cohort, grain, currency và maturity. Không dùng `amount retention` đồng thời với `Average Net Final Amount` nếu làm double-count. Một cách thay thế là:

```text
Expected Final Amount
= Expected Valid Provisional Commission Amount
× Amount Retention Rate
```

### Break-even phải ghi basis

```text
Economic break-even matured clicks
= Applicable economic cost / Final EPC

Economic break-even qualified sessions
= Applicable economic cost / Final RPV

Cash break-even qualified sessions
= Allocated non-embedded cash outflow cost / Paid RPV
```

Nếu EPC/RPV chưa đủ maturity hoặc bằng 0, kết quả là `UNKNOWN`; không phải vô hạn hay mô hình thất bại.

### Ví dụ minh họa, không phải benchmark

```text
Qualified sessions của cohort: 2.000
Matured network accepted clicks: 120
Net Final Commission: 96 USD
Net paid cash đã allocation về cohort: 80 USD
Allocated cash operating cost chưa embedded trong received net: 40 USD
Imputed labor cost: 80 USD
```

```text
Final EPC = 96 / 120 = 0,80 USD
Final RPV = 96 / 2.000 = 0,048 USD

Cash contribution = 80 - 40 = 40 USD
Cash Profit/1.000 Qualified Sessions = 20 USD

Economic contribution = 96 - 40 - 80 = -24 USD
Economic Profit/1.000 Qualified Sessions = -12 USD
```

Cash dương có thể vẫn economic âm; Final và Paid không được thay cho nhau.

## 8. KPI chất lượng, ngưỡng và minimum sample

| KPI | Công thức | Rule ban đầu |
|---|---|---|
| Critical Evidence Coverage | Critical public fields có evidence / tổng critical public fields | 100% từng page |
| Critical Freshness Coverage | Critical public fields trong SLA / tổng critical public fields | 100% từng page; stale phải HOLD/ẩn |
| Disclosure Coverage | Affiliate pages pass / affiliate pages | 100% |
| Approved Paid-Link Rel Coverage | Affiliate anchors có qualifying rel đã duyệt / affiliate anchors | 100%; mặc định `sponsored`, ngoại lệ `nofollow` có evidence |
| Synthetic Event Completeness | Complete synthetic events / expected synthetic events | 100% |
| Synthetic Duplicate Rate | Duplicate synthetic events / expected synthetic events | 0% |
| Production Event Completeness | Complete events / sampled production events | Chỉ rate khi N ≥500; trước đó hiển thị count |
| Production Event Duplicate Rate | Duplicates / sampled production events | Chỉ rate khi N ≥500 |
| Critical Job Failure | Failed critical jobs | Bất kỳ unresolved failure nào là đỏ |
| Job Success Rate | Successful jobs / scheduled jobs | Chỉ trend khi ≥30 executions |
| Recommendation Reproducibility | Checked runs tái tạo cùng ranking/cost/reason IDs / checked runs | 100% |
| Human Intervention | Unscheduled corrective interventions / auto-runs | Hiển thị count trước 100 runs |
| Reconciliation Difference | abs(expected − observed) theo cùng basis/currency | Theo configured materiality |
| MTTR | Trung bình restore_at − detected_at theo severity | Theo internal severity SLA |

Một critical claim thiếu evidence, affiliate page thiếu disclosure hoặc unsafe destination là `HOLD/NO-PUBLISH`; không dùng aggregate percentage để che.

### Minimum sample để đọc business pattern

| Metric | Chỉ bắt đầu phân tích pattern khi | Trước ngưỡng |
|---|---:|---|
| Utility Completion | khoảng ≥100 starts | Debug/usability, ghi LEARNING |
| Result-to-Outbound-Click | khoảng ≥100 result-viewing sessions | Chỉ mô tả |
| Click Conversion | ≥100 matured accepted clicks **và** ≥5 attributed orders | Range/unknown; ưu tiên tracking QA |
| EPC/RPV | ≥100 matured clicks và policy windows đủ tuổi | Không dùng để scale budget |
| A/B decision | Pre-registered sample/time | INCONCLUSIVE nếu thiếu |

Các số trên là analysis hints, không phải benchmark ngành hoặc cam kết hiệu suất.

## 9. Ba dashboard tách biệt

### Activity Snapshot — hằng ngày/tuần

Dùng [15a Activity Snapshot](templates/15a-activity-snapshot.csv) cho Search, qualified sessions, utility, result, outbound click, event quality, jobs, evidence/freshness/disclosure. Synthetic và production event QA có operands/rate riêng; QA event phải đọc theo `event_name + event_version`, không chỉ aggregate `ALL`. Soft launch lưu `NOT_APPLICABLE_NOT_MONETIZED` cùng test-plan reference. Đây là calendar activity; không chứa click-to-order conclusion.

### Cohort Funnel — khi cohort đủ tuổi

Dùng [15b Cohort Funnel](templates/15b-cohort-funnel.csv) theo program + click cohort. Lưu raw operands cho matured clicks, distinct converted clicks, order lines, eligible valid/final lines, gross/reversed/net final amounts, match counts và allocated cohort costs. Mọi amount trong snapshot đã quy đổi về `reporting_currency`; raw currency/FX evidence vẫn ở ledger.

### Payout Snapshot — theo payout period/due cutoff

Dùng [15c Payout Snapshot](templates/15c-payout-snapshot.csv) cho final gross due, statement gross/fee/net, allocations, net received, materiality, grace/overdue và cash contribution. Mọi amount trong snapshot dùng `reporting_currency`; raw statement/receipt currencies và từng FX source/date vẫn ở Payouts/Allocations. Không tính Paid RPV nếu allocation về cohort chưa đủ.

## 10. Đối soát sai lệch

| Hiện tượng | Kiểm tra trước |
|---|---|
| Browser event > router click | Direct links, event phát trước router, duplicate, non-router links, bot/internal |
| Router click > network click | Network dedupe/filter, delay, wrong ID/link, program rule |
| Network order không có exact click | Không trả sub-ID, cross-device, malformed ID, delayed import |
| Final thấp hơn Valid | Reversal/refund, grain hoặc currency mapping |
| Statement gross thấp hơn Final due | Threshold/schedule, hold/KYC, allocation/mapping |
| Net received thấp hơn statement gross | Expected fees, FX/transfer fee hoặc payment discrepancy |

Mỗi discrepancy phải có program, grain, timezone, data window, maturity và source references. Không sửa số thủ công để “khớp”.

## 11. Các lag phải tách

- `Click-to-Order Lag = order_occurred_at − accepted_click_at`.
- `Order-to-Final Lag = final_effective_at − order_occurred_at`.
- `Final-to-Payout-Due Lag = payout_due_at − final_effective_at`.
- `Payout-Due-to-Received Lag = payout_received_at − payout_due_at`; có thể âm nếu trả sớm.
- `Click-to-Cash Lag = payout_received_at − accepted_click_at`, chỉ khi allocation/match đủ.
- `Pending Aging = as_of_at − order_reported_at` cho order còn pending.

`accepted_click_at` là timestamp do network chấp nhận nếu network cung cấp; không thay âm thầm bằng browser event time. Thiếu timestamp/match cần thiết thì lag là `UNKNOWN`. Không dùng một cột “Payout Lag từ order/click theo định nghĩa” vì không so sánh được giữa kỳ.

## 12. Search Console dùng đúng cách

Search Console có definitions và aggregation riêng. Dùng để xác minh property, theo dõi crawl/index, sitemap và query/page/country/device trends; không coi submit là bảo đảm index. Xem [Getting started](https://support.google.com/webmasters/answer/10267942) và [impression/click/position definitions](https://support.google.com/webmasters/answer/7042828).

## 13. Weekly KPI narrative mẫu

```text
Activity window:
Click cohort/program:
Attribution/validation/final maturity:
Payout due cutoff/grace:
Formula version:
Data quality status:

FACTS
- Discovery/activity:
- Utility:
- Affiliate cohort:
- Final/payout:
- Costs/operations:

INFERENCES:
UNKNOWNS / NOT_APPLICABLE:
GUARDRAILS:
DECISION:
- Maintain / Iterate / Hold / Stop / Scale-candidate
- Owner, next action, review date
```

Số liệu thiếu grain, raw numerator/denominator, maturity hoặc source reference chưa đủ để ra quyết định.

