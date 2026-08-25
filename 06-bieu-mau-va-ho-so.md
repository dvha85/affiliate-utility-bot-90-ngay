# 06 — Biểu mẫu và hồ sơ bằng chứng

## 1. Cấu trúc hồ sơ dự án đề xuất

Đây là cấu trúc logic; bạn có thể dùng thư mục máy tính, Drive hoặc công cụ quản lý khác. Không đưa file bí mật vào nơi public.

```text
AffiliateUtilityBot/
├─ 00-charter-decisions/
│  ├─ project-charter.md
│  └─ decision-log.csv
├─ 01-research/
│  ├─ research-log.csv
│  ├─ interview-notes/
│  ├─ opportunity-cards/
│  └─ competitor-observations/
├─ 02-programs/
│  ├─ program-reviews/
│  ├─ program-registry.csv
│  └─ application-status/       # không lưu KYC/bank/secrets
├─ 03-data/
│  ├─ source-registry.csv
│  ├─ products.csv
│  ├─ plans.csv
│  ├─ feature-evidence.csv
│  ├─ source-evidence.csv
│  ├─ dataset-release-manifest.csv
│  ├─ link-registry.csv
│  └─ changelog/
├─ 04-product-ux/
│  ├─ user-flow/
│  ├─ scoring-spec/
│  ├─ scoring-rules.csv
│  ├─ stack-rules.md
│  ├─ recommendation-component-scores.csv
│  ├─ golden-tests.csv
│  └─ usability-notes/
├─ 05-content/
│  ├─ content-inventory.csv
│  ├─ briefs/
│  └─ publishing-approvals/
├─ 06-analytics-finance/
│  ├─ event-contract.csv
│  ├─ event-click-log.csv
│  ├─ network-status-mapping.csv
│  ├─ activity-snapshot.csv
│  ├─ cohort-funnel.csv
│  ├─ payout-snapshot.csv
│  ├─ orders.csv
│  ├─ commission-transitions.csv
│  ├─ payouts.csv
│  ├─ payout-allocations.csv
│  └─ cost-log.csv
├─ 07-operations/
│  ├─ run-logs/
│  ├─ weekly-reviews/
│  ├─ experiments/
│  └─ incidents/
└─ 99-private-outside-public-repo/
   ├─ secrets/                  # dùng secret manager, không file text nếu có thể
   ├─ kyc-tax/
   └─ payment-evidence/
```

Thư mục `99-private` phải nằm ngoài website/repo public và có quyền truy cập tối thiểu. Trong ledger công khai nội bộ chỉ lưu `payment_evidence_reference`, không chép số tài khoản hay ảnh giấy tờ.

## 2. Quy tắc đặt tên và version

### Timestamp

Dùng ISO 8601 và ghi timezone, ví dụ:

```text
2026-08-25T14:30:00+07:00
```

Hệ thống có thể lưu UTC (`Z`) và dashboard hiển thị Asia/Ho_Chi_Minh, nhưng không trộn hai timezone mà thiếu nhãn.

### Version

```text
data-v0.1.0
scoring-v0.1.0
page-v0.1.0
event-contract-v0.1.0
```

- Tăng `patch` khi sửa nhỏ không đổi nghĩa.
- Tăng `minor` khi thêm field/rule có tương thích.
- Tăng `major` khi cách tính/kết quả/contract thay đổi đáng kể.
- Mọi Recommendation Run lưu cả data version và scoring version.

### ID namespace

Không dùng cùng `run_id` cho bot job và một lần recommendation:

```text
JOB-...  job/bot run
REC-...  recommendation run
JNY-...  một journey giả danh
EVT-...  event
CLK-...  outbound click
LNK-...  stable public link; `link_version` đổi khi destination/rule đổi
OFF-...  commercial offer; tách khỏi link/version
IMP-...  import batch
ORD-...  normalized order
OEV-...  order observation/event
COM-...  commission
CTR-...  commission transition
PAY-...  payout
PAL-...  payout allocation record
CST-...  cost entry
CAL-...  cost allocation row
CHG-...  change request
REL-...  release
```

ID phải duy nhất, ổn định và không chứa email, tên, số điện thoại hay customer ID thô. Business ID (`ORD/COM/PAY/CST`) giữ nguyên qua nhiều observation/version; record ID (`OEV/CTR/PAL/CAL`) là duy nhất cho từng dòng append-only.

### CSV có field đa giá trị

Các cột như candidate IDs, reasons hoặc required parameters phải dùng JSON array được quote đúng chuẩn CSV hoặc tách child table; không dùng dấu phẩy tự do làm hỏng cột. ID và ISO timestamp nên import vào Excel/Sheets dưới dạng text. File xuất lại phải là UTF-8.

### Trạng thái dữ liệu

```text
DRAFT → STAGING → APPROVED → PUBLISHED
                    ↓
              STALE / CONFLICT / RETIRED
```

Không ghi đè mất lịch sử. Sửa sai bằng version/change record mới.

## 3. Biểu mẫu nào là nguồn sự thật?

| Nội dung | Source of truth nội bộ | Không dùng thay thế |
|---|---|---|
| Mục tiêu/phạm vi | Project Charter + Decision Log | Tin nhắn rời rạc |
| Nhu cầu người dùng | Research Log | Cảm nhận của bot |
| Quyền dùng nguồn | Source Registry + terms evidence | Chỉ robots.txt |
| Giá/tính năng | Product/Plan/Feature Evidence | Trí nhớ LLM/blog tổng hợp |
| Affiliate rules | Program Review + official agreement | Commission landing page riêng lẻ |
| Link đang public | Link Registry | Link hard-code trong bài |
| Expected recommendation | Golden Tests | Kết quả hiện tại của engine |
| Trang được phép publish | Publishing Approval | “Đã đọc qua” |
| Hành vi trên site | Event/Click Log + analytics; router log nếu có | Search Console riêng |
| Order/commission | Network export/API + Orders + Commission Transitions | Web analytics click |
| Payment | Payouts + Payout Allocations + bằng chứng thực nhận | Final commission hoặc statement riêng lẻ |
| Quyết định | Decision Log | KPI dashboard không có diễn giải |

## 4. Thứ tự tạo biểu mẫu trong 90 ngày

### Day 1–7

1. Sao chép [Project Charter](templates/01-project-charter.md).
2. Tạo [Decision Log](templates/16-decision-log.csv).
3. Tạo [Cost Log](templates/19-cost-log.csv), kể cả khoản 0 đồng nhưng tốn giờ.

### Day 8–21

4. Tạo [Research Log](templates/02-research-log.csv).
5. Tạo một [Opportunity Card](templates/03-opportunity-card.md) cho mỗi cluster vào shortlist.

### Day 22–35

6. Tạo [Affiliate Program Review](templates/04-affiliate-program-review.md) và [Program Registry](templates/04b-program-registry.csv) theo program.
7. Tạo [Source Registry](templates/06-source-registry.csv) và [Source Evidence](templates/07d-source-evidence.csv).
8. Tạo [Products](templates/07a-products.csv), [Plans](templates/07b-plans.csv) và [Feature Evidence](templates/07c-feature-evidence.csv).
9. Viết [Scoring Rules](templates/24-scoring-rules.csv), [Stack Rules](templates/29-stack-rules.md), tạo [Golden Tests](templates/05-golden-tests.csv) trước khi code và khóa [Dataset Release Manifest](templates/25-dataset-release-manifest.csv). [Recommendation Component Scores](templates/26-recommendation-component-scores.csv) là bảng đích cho từng lần tính/kiểm thử.

### Day 36–70

10. Tạo [Link Registry](templates/08-link-registry.csv).
11. Tạo [Data/Privacy Inventory](templates/18-data-privacy-inventory.csv).
12. Tạo [Event Contract](templates/21-event-contract.csv), [Recommendation Run Log](templates/22-recommendation-run.csv) và [Event/Click Log](templates/27-event-click-log.csv).
13. Tạo [Content Inventory](templates/20-content-inventory.csv), [Content Brief](templates/17-content-brief.md) và [Publishing Approval](templates/09-publishing-approval.md).
14. Tạo [Activity Snapshot](templates/15a-activity-snapshot.csv), [Cohort Funnel](templates/15b-cohort-funnel.csv), [Payout Snapshot](templates/15c-payout-snapshot.csv) và dùng [Change Request](templates/23-change-request.md) cho thay đổi material.

### Day 71–90 và sau launch

15. Mỗi thử nghiệm có [Experiment Card](templates/10-experiment-card.md) trước khi chạy.
16. Mỗi tuần có [Weekly Review](templates/11-weekly-review.md).
17. Mỗi job có [Daily Run Log](templates/12-daily-run-log.md).
18. Khi network có dữ liệu thật, phê duyệt [Network Status Mapping](templates/28-network-status-mapping.csv) trước lần import đầu; sau đó dùng [Orders](templates/14a-orders.csv), [Commission Transitions](templates/14b-commission-transitions.csv), [Payouts](templates/14c-payouts.csv) và [Payout Allocations](templates/14d-payout-allocations.csv).
19. Mỗi sự cố có [Incident Report](templates/13-incident-report.md).

## 5. Cách điền Research Log đúng

### Một evidence record tốt trả lời

- Ai gặp vấn đề?
- Trong hoàn cảnh nào?
- Họ đang cố hoàn thành việc gì?
- Câu/tín hiệu quan sát được là gì?
- Đây là nhu cầu, phàn nàn, so sánh hay ý định mua?
- Nguồn ở đâu và quan sát/thu thập khi nào?
- Nó ủng hộ hay phản bác giả thuyết?
- Có được phép lưu/tái sử dụng phần nào?

### Không làm

- Không chép cả bài/post/review vào sheet.
- Không thu username/email nếu không cần.
- Không biến một bài đăng thành “30 bằng chứng” bằng cách tách câu.
- Không chỉ giữ câu đồng ý với ý tưởng.
- Không coi autocomplete/search result count là doanh thu.

### Confidence

- `A`: hành vi/tuyên bố rõ, đúng segment/context, source đáng tin.
- `B`: liên quan nhưng context/segment chưa chắc.
- `C`: tín hiệu để nghiên cứu tiếp, không dùng một mình để quyết định.

## 6. Cách quản lý price và plan trong bảng tính

Ví dụ minh họa cách ghi, không phải giá thật:

```text
actual_charge_amount = 120
billing_interval = YEAR
annual_equivalent_monthly_amount = 10
currency = USD
```

UI phải nói “120 USD thanh toán hằng năm, tương đương 10 USD/tháng”, không nói “10 USD/tháng” nếu người mua phải trả 120 USD một lần.

Các giá trị khác nhau theo region phải là rows riêng. `UNKNOWN` khác 0; `NOT_AVAILABLE` khác `UNKNOWN`.

## 7. Cách ghi evidence theo field

Một product page URL không tự động chứng minh mọi field. Mỗi critical field cần:

```text
field_or_feature_key: monthly_price
normalized_value: 29
value_type: MONEY
source_id: SRC-...
source_url: ...
evidence_summary: Trang pricing ghi gói X có giá ... tại region ...
checked_at: ...
effective_at: UNKNOWN hoặc ngày nguồn nêu
next_check_at: ...
confidence: A
status: VERIFIED
```

Không cần quote dài; summary phải đủ để reviewer tìm lại.

## 8. Link Registry không phải kho bí mật tùy tiện

Affiliate URL đôi khi chứa identifier gắn với tài khoản. Quyết định rõ:

- Nếu URL được thiết kế để public trên website, có thể lưu theo repository policy đã duyệt.
- Nếu credential/token/API secret, chỉ lưu `secret_reference`, không lưu value trong CSV.
- Link có thể là `DIRECT` hoặc `ROUTER`; router là optional và chỉ dùng khi program cho phép.
- Nếu dùng router, router chỉ nhận `link_id` allowlisted rồi lookup active `link_version`; không nhận arbitrary destination URL từ user, tránh open redirect.
- Mọi product link phát một `product_outbound_click`; affiliate click được suy ra từ `link_type=AFFILIATE + link_id/link_version active`, không phát event thứ hai. `delivery_mode` chỉ là `DIRECT / ROUTER`, không quyết định link có phải affiliate hay không.
- Mọi offer có official/non-affiliate fallback rõ khi program paused.

Soft launch dùng non-affiliate links ghi revenue reconciliation là `NOT_APPLICABLE_NOT_MONETIZED` và lưu `monetization_test_plan_reference` trong Activity Snapshot/decision record cho lúc program approved; không điền 0 vào order/final/paid để giả rằng pipeline monetization đã được thử.

## 9. Ledger tiền là append-only

Không sửa một order/commission từ `PENDING` thành `FINAL` bằng cách mất dấu trạng thái cũ. Mỗi import/status observation và commission transition là record mới:

- [Orders](templates/14a-orders.csv): order events/observations, original status, canonical status và attribution.
- [Commission Transitions](templates/14b-commission-transitions.csv): `from/to`, amount, maturity và reversal.
- [Payouts](templates/14c-payouts.csv): statement gross/fee/net và tiền thực nhận.
- [Payout Allocations](templates/14d-payout-allocations.csv): quan hệ nhiều-nhiều giữa payout và commission.
- [Network Status Mapping](templates/28-network-status-mapping.csv): định nghĩa vì sao status nguồn map sang canonical state.

Không cộng thẳng toàn bộ các dòng lịch sử:

- `order_amount` là snapshot của một observation; current order view chọn observation hiệu lực mới nhất theo order/line và cutoff.
- Commission transition phải khai `amount_semantics`: `SNAPSHOT` thì current view lấy transition hiệu lực mới nhất; `DELTA` thì cộng signed delta theo policy. Không được cộng lặp mọi snapshot.
- Payout đổi statement/receipt phải append `record_version` mới trỏ version cũ; báo cáo chỉ dùng version hiện hành tại `as_of_at`.
- Payout allocation sửa sai bằng dòng `REVERSE` âm trỏ allocation cũ rồi dòng `ALLOCATE` mới; không sửa hoặc xóa dòng đã đối soát.

`PAID` là kết quả payout/allocation, không ghi đè commission `FINAL`. Bốn số bắt buộc tách:

```text
Ordered Commission: network vừa báo theo order
Valid Commission: order đã được network chấp nhận ở bước valid
Final Commission: commission được chốt theo định nghĩa program
Paid Cash: số tiền thực nhận và đã đối soát
```

Khi không nối được conversion với click bằng ID được hỗ trợ, ghi `UNMATCHED`; không gọi việc suy theo sản phẩm/ngày là `EXACT`.

Đối soát payout phải tách:

```text
Gross payout coverage = allocated gross / final gross due
Net cash realization = received net / expected net after known fees
```

Hai vế của mỗi công thức phải cùng currency/basis. Tính chênh lệch/materiality riêng cho gross allocation và net receipt; không gộp hai basis vào một difference. Mỗi chênh lệch mở exception khi vượt `max(materiality tuyệt đối theo currency, expected × materiality %)` hoặc khi expected bằng 0 nhưng có amount không giải thích được.

[Cost Log](templates/19-cost-log.csv) dùng một `cost_entry_id` cho khoản gốc và một `cost_allocation_id` cho mỗi lát phân bổ. Khi một khoản chia cho nhiều program/page/cohort, tổng `allocation_fraction` của allocation set phải bằng 1; dashboard cộng allocated amounts, không cộng lại khoản gốc lặp trên nhiều dòng. `cash_treatment` phải chỉ rõ phí là outflow riêng hay đã bị trừ trong payout net: economic view vẫn tính chi phí liên quan, còn cash contribution không trừ lần hai khoản `EMBEDDED_IN_RECEIVED_NET`.

## 10. Hồ sơ tối thiểu để audit một recommendation

Trong [Recommendation Run Log](templates/22-recommendation-run.csv), grain là đúng **một dòng cho một `REC-` run**. Candidate/reason fields dùng JSON array hoặc child table. Không nhét nhiều click vào dòng này: một run có thể sinh 0..n `product_outbound_click`, mỗi event nằm ở Event/Click Log và nối bằng `recommendation_run_id`. Chỉ giữ input tối thiểu theo `input_storage_mode`, privacy classification và deletion due date; không log PII/free text chỉ để debug.

Từ một kết quả đã hiển thị, bạn phải truy lại được:

```text
recommendation_run_id
→ journey_id
→ input chuẩn hóa
→ data_version
→ scoring_version
→ candidates và hard-filter reasons
→ component scores
→ evidence IDs cho price/features/limitations
→ output và explanation
→ page version
→ Event/Click Log nối bằng recommendation_run_id
→ một hoặc nhiều link_id + link_version được click
→ offer_id + offer_version liên quan nếu link là affiliate
```

Nếu không truy lại được, hệ thống chưa đủ điều kiện auto-update ranking.

## 11. Retention và xóa dữ liệu

Không đặt một thời hạn retention chung cho mọi dữ liệu. Trong [Data/Privacy Inventory](templates/18-data-privacy-inventory.csv), mỗi item phải có:

- Mục đích cụ thể.
- Data fields/categories.
- Có phải personal/sensitive data không.
- Người nhận/processor và nơi lưu.
- Consent/lawful-basis review reference theo thị trường.
- Thời hạn giữ và lý do.
- Cách xóa/ẩn danh.
- Quy trình quyền của người dùng.
- Owner và ngày review.

Nếu không giải thích được vì sao cần một field cá nhân, không thu field đó trong MVP.

## 12. Definition of Done cho hồ sơ

- [ ] File mở được và encoding đúng.
- [ ] ID duy nhất và liên kết tham chiếu tồn tại.
- [ ] Timestamp/timezone rõ.
- [ ] Không secret/direct identifier/Google-prohibited PII; personal data giả danh chỉ tồn tại trong phạm vi inventory, mục đích, quyền truy cập và retention đã duyệt.
- [ ] Critical claims có source.
- [ ] Unknown/conflict không bị che.
- [ ] Reviewer/owner/date có đủ.
- [ ] Version/change/rollback rõ khi ảnh hưởng production.
- [ ] Có next action hoặc next review date.

