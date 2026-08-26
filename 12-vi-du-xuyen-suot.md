# 12 — Ví dụ xuyên suốt đã điền: từ giả thuyết đến payout

> Toàn bộ tên sản phẩm, URL, giá, chương trình và số tiền dưới đây là **dữ liệu hư cấu chỉ để học**. Không copy chúng vào website thật. Khi làm dự án, thay từng dòng bằng bằng chứng chính thức và ngày kiểm tra thực tế.

## 1. Tình huống mẫu

Lan xây Website Stack Builder tiếng Việt cho freelancer lần đầu cần website nhận khách. Utility cần trả lời:

> “Tôi là freelancer chụp ảnh mới, có tối đa 60 USD/tháng và không biết code; tôi nên chọn domain, hosting và cách dựng landing page nào để nhận liên hệ?”

Project Charter rút gọn:

| Field | Giá trị mẫu |
|---|---|
| Segment | Freelancer Việt Nam lần đầu cần website nhận khách |
| JTBD | Chọn domain, hosting và cách dựng landing page để giới thiệu dịch vụ/nhận liên hệ |
| Market/language | Việt Nam / tiếng Việt |
| Team size | 1 |
| Billing preference | Monthly only |
| Max monthly budget | 60 USD |
| Max upfront payment | 60 USD |
| Phase 1 | LEAN: 8 sản phẩm, 3 decision assets |
| Link mode lúc soft-launch | Direct official non-affiliate |
| Change authority Day 90 | L2; L3 chỉ là stretch sau đủ gate |

Điều chưa biết không được đoán. Ví dụ: `Affiliate program availability = UNKNOWN; owner=Lan; review_at=Day 24`.

## 2. Bằng chứng nhu cầu

Ba dòng minh họa trong [Research Log](templates/02-research-log.csv):

| ID | Evidence hư cấu | Insight chuẩn hóa | Không được kết luận |
|---|---|---|---|
| RES-001 | Một câu hỏi về “làm portfolio dưới ngân sách thấp” | Có ràng buộc ngân sách và kỹ năng | “Thị trường chắc chắn lớn” |
| RES-002 | Một người than khó so giá hosting theo năm với giá tháng | Cần tách actual charge và monthly equivalent | “Mọi người chỉ muốn monthly” |
| RES-003 | Một comparison page đang xếp hạng | Có intent so sánh | “Trang đó kiếm được bao nhiêu tiền” |

Sau 30 evidence, chỉ giữ insight xuất hiện lặp lại và còn được nguồn trực tiếp hỗ trợ. Không lấy ba dòng ví dụ này làm bằng chứng đủ để qua Gate 1.

## 3. Program và link mode

Ngày 24, chương trình hư cấu chưa duyệt:

```text
Program ID: PGM-DEMO-001
Decision: HOLD
Program Status: PENDING_APPLICATION
Reason: chưa xác nhận target country và organic-content channel
Public link mode: OFFICIAL_NON_AFFILIATE
Reconciliation: NOT_APPLICABLE_YET
Next action: owner đọc network agreement + vendor channel terms
```

Kết luận đúng là **soft-launch không affiliate**, không phải tự đoán điều khoản và cũng không cần chặn toàn bộ việc xây utility.

## 4. Dataset hư cấu

### Sản phẩm và plan

| Product | Category/coverage | Actual charge | Monthly equivalent | Limitation |
|---|---|---:|---:|---|
| `PRD-DEMO-HOST` | Hosting + SSL | 29 USD mỗi tháng | 29 USD | Chưa gồm domain/website builder |
| `PRD-DEMO-DOMAIN` | Domain + email forward | 19 USD mỗi tháng | 19 USD | Chưa có hosting/landing page |
| `PRD-DEMO-BUNDLE` | Domain + hosting + no-code landing page | 39 USD mỗi tháng | 39 USD | Khả năng tùy chỉnh hạn chế |
| `PRD-DEMO-ANNUAL` | Domain + hosting + WordPress | 360 USD trả trước/năm | 30 USD | Annual-only; vượt max upfront |

Mỗi field giá/feature thật cần một `source_id`, URL chính thức, `checked_at`, `next_check_at`, source tier và confidence. Dòng annual phải giữ cả `actual_charge=360` và `monthly_equivalent=30`; không được nói người dùng chỉ phải trả 30 USD khi checkout.

### Release có thể tái tạo

```text
Release: REL-20260825-001
Dataset version: data-0.1.0
Scoring version: score-0.1.0
Products/plans/evidence: exact record IDs + versions trong manifest
Approved change: CHG-20260825-001
```

Không chỉ ghi “dùng dữ liệu mới nhất”; recommendation phải trỏ tới release cụ thể.

## 5. Hard filter và scoring

Input mẫu:

```json
{
  "market": "VN",
  "currency": "USD",
  "team_size": 1,
  "monthly_budget": 60,
  "billing_preference": "MONTHLY_ONLY",
  "max_upfront_payment": 60,
  "must_have_needs": ["DOMAIN", "HOSTING", "LANDING_PAGE"]
}
```

Kết quả hard filter:

| Candidate stack | Hard-filter result | Lý do |
|---|---|---|
| HOST + DOMAIN | PASS | Phủ đủ domain/hosting; cần thêm cách dựng landing page miễn phí đã duyệt |
| BUNDLE | PASS | Phủ đủ ba need; actual charge 39 USD |
| ANNUAL | REJECT | Actual upfront 360 USD > 60 USD và không monthly |

Điểm component minh họa theo rubric đã version:

| Stack | Need fit | Budget fit | Ease | Limitations | Evidence | Stack Fit 0–100 |
|---|---:|---:|---:|---:|---:|---:|
| HOST + DOMAIN | 4 | 4 | 4 | 4 | 5 | 84 |
| BUNDLE | 5 | 5 | 5 | 3 | 4 | 90 |

`90` chỉ là điểm tương đối trong `data-0.1.0 + score-0.1.0`, không phải xác suất thành công hay xếp hạng tuyệt đối của thị trường.

Output deterministic:

1. **Best Fit:** BUNDLE — phủ đủ domain/hosting/landing page, evidence mạnh, 39 USD/tháng.
2. **Cheapest Qualified:** HOST + DOMAIN — 48 USD/tháng, cần dùng landing-page method miễn phí đã được mô tả rõ và có giới hạn tùy chỉnh.
3. **Alternative:** không có candidate khác biệt đủ ý nghĩa → hiển thị hai phương án, không bịa phương án thứ ba.

Commission không được dùng để chọn, sắp xếp, làm nổi bật hoặc phá hòa hai kết quả.

## 6. Golden Tests trước khi có giao diện

Tối thiểu thêm các ca sau vào [Golden Tests](templates/05-golden-tests.csv):

| Test | Input thay đổi | Expected |
|---|---|---|
| G-001 | Input chuẩn ở trên | Best Fit BUNDLE; Cheapest HOST+DOMAIN |
| G-002 | Budget 35 USD | No valid stack + giải thích thiếu ngân sách |
| G-003 | Monthly-only | ANNUAL luôn bị reject |
| G-004 | Thiếu price evidence của BUNDLE | BUNDLE bị HOLD/ẩn claim theo rule |
| G-005 | Hai stack bằng điểm | Tie theo evidence → monthly cost → actual/upfront charge → fewer tools → stable ID/equal rank |
| G-006 | `must_have_needs=[]` | Validation error; không chạy scoring |

Pass cứng là 100% hard constraints. Soft ordering được review theo threshold starter đã viết trước và không thay sau khi nhìn kết quả để “làm đẹp” số.

## 7. Một recommendation run và nhiều click

Khi mở utility:

```text
journey_id = JNY-20260825-001
event = utility_start
```

Khi submit input hợp lệ:

```text
recommendation_run_id = REC-20260825-001
data_version = data-0.1.0
scoring_version = score-0.1.0
output = BUNDLE / HOST+DOMAIN
```

Nếu người dùng bấm cả hai kết quả, đây là **hai event**, không phải hai cột được nhét vào Recommendation Run:

| Event ID | Run | Link type | Product | Derived affiliate click |
|---|---|---|---|---|
| EVT-001 | REC-20260825-001 | OFFICIAL_NON_AFFILIATE | PRD-DEMO-HOST | false |
| EVT-002 | REC-20260825-001 | OFFICIAL_NON_AFFILIATE | PRD-DEMO-BUNDLE | false |

GA enhanced measurement có thể hỗ trợ một số external click, nhưng dự án vẫn dùng custom `product_outbound_click`. Nếu link đi qua `/go/...` cùng domain thì không được giả định GA tự nhận đó là outbound click.

## 8. Chuyển sang affiliate có kiểm soát

Khi program được duyệt, không sửa URL trực tiếp trong trang. Tạo Change Request:

```text
Change: CHG-20260910-002
Before: link_type=OFFICIAL_NON_AFFILIATE
After: link_type=AFFILIATE, offer_version=2
Evidence: approved agreement/version + dashboard link format
Tests: destination allowlist, disclosure, qualifying rel, custom event, rollback
Decision: APPROVE bởi human owner
```

Từ release đó, `product_outbound_click` có `link_type=AFFILIATE` mới được suy ra thành `affiliate_click` và đưa vào reconciliation.

## 9. Order, commission transition và payout

Ví dụ hư cấu sau khi cohort đã đủ maturity:

```text
Click: CLK-001
Order line: ORD-001-L1; order value = 100 USD
Commission COM-001:
  PENDING 20 USD
  → VALID 20 USD
  → FINAL 16 USD (4 USD reversal/adjustment có reason)
Payout PAY-001:
  gross allocated = 16 USD
  documented fee = 0.80 USD
  net received allocated = 15.20 USD
```

Không ghi đè status cũ. Mỗi transition là một dòng append-only; payout và allocation là bảng riêng để hỗ trợ nhiều commission trong một payout hoặc trả từng phần.

## 10. KPI cohort đúng mẫu số

Giả sử một click cohort đã mature có:

- 100 network-accepted affiliate clicks đã mature.
- 5 distinct click IDs có ít nhất một order.
- 5 attributed order lines; 4 eligible valid lines; 3 finalized lines.
- Average final amount trên finalized order line: 16 USD.

```text
Click Conversion Rate = 5 / 100 = 5%
Validation Rate = 4 / 5 = 80%
Finalization Rate = 3 / 4 = 75%
Expected Final Amount
  = 100 × 5% × 80% × 75% × 16
  = 48 USD
```

Không lấy click tuần này chia cho order tuần này nếu order đến từ click cũ. Activity snapshot, click cohort và payout snapshot được giữ riêng.

Nếu `Paid cash=40`, `Final commission=48`, `cash cost=20`, `imputed labor=50` và cohort có 1.000 Qualified Sessions:

```text
Cash contribution = 40 - 20 = 20 USD
Cash Profit/1.000 Qualified Sessions = 20 USD
Economic contribution = 48 - 20 - 50 = -22 USD
Economic Profit/1.000 Qualified Sessions = -22 USD
```

Một cohort có tiền về vẫn có thể chưa hiệu quả kinh tế sau khi tính thời gian.

## 11. Quyết định cuối chu kỳ

Ví dụ kết luận đúng:

```text
SYSTEM: PASS
MARKET: NOT_ENOUGH_DATA
REVENUE: NOT_ENOUGH_DATA
AUTOMATION: KEEP-L2
DECISION: MAINTAIN + thu thêm dữ liệu 30 ngày
```

Không kết luận “thất bại” chỉ vì chưa có organic order trong một cửa sổ ngắn; cũng không gọi `PENDING` là doanh thu đã kiếm được.

## 12. Checklist tự làm lại ví dụ

- [ ] Copy [Project Charter](templates/01-project-charter.md) và thay toàn bộ dữ liệu hư cấu.
- [ ] Có ít nhất một research record với source thật và timestamp.
- [ ] Chọn direct non-affiliate hoặc affiliate dựa trên Program Review, không dựa trên mong muốn.
- [ ] Tạo product/plan/evidence records có source.
- [ ] Khóa dataset release, scoring rules và stack rules.
- [ ] Tính tay một Golden Test trước khi viết giao diện.
- [ ] Tái tạo cùng kết quả từ cùng input/data/scoring version.
- [ ] Bắn custom event và kiểm tra `journey_id`/`recommendation_run_id`/`outbound_click_id`.
- [ ] Nếu chưa có affiliate active, ghi reconciliation `NOT_APPLICABLE_YET + test plan`.
- [ ] Chỉ mở affiliate tracking sau human approval và full QA.

