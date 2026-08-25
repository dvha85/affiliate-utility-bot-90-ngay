# 08 — Cổng Go/No-Go và quy tắc Kill/Maintain/Scale

## 1. Cách dùng gate

Mỗi gate trả lời một câu hỏi duy nhất. Không vượt gate bằng nhiệt tình, số trang hay tiền đã bỏ ra.

Kết quả hợp lệ:

- `GO`: đủ điều kiện chuyển bước.
- `GO-LEAN`: tiếp tục với phạm vi nhỏ hơn đã ghi.
- `CONDITIONAL`: blocker bên ngoài đang chờ nhưng có đường an toàn tiếp tục.
- `REWORK`: sửa trong tối đa 7 ngày, rồi chạy lại cùng tiêu chí.
- `PIVOT`: đổi segment/problem/method, quay lại gate liên quan.
- `HOLD`: giữ hệ thống an toàn, chờ dữ liệu/sự kiện ngoài.
- `NO-LAUNCH/NO-PUBLISH/NO-AUTO`: không cấp quyền tương ứng.
- `STOP`: dừng vì rủi ro, không khả thi hoặc không còn phù hợp.

### Quy tắc chống tự lừa mình

1. Viết tiêu chí trước khi nhìn kết quả.
2. Không thay mẫu số/window/maturity để làm KPI đẹp hơn.
3. Guardrail fail luôn chặn growth gate.
4. `NOT_ENOUGH_DATA` khác `FAIL`.
5. Sunk cost không phải lý do GO.
6. Mỗi `GO` có reviewer, evidence, ngày và next review.

Các số lượng/tỷ lệ/thời gian dưới đây là **starter thresholds nội bộ của bài tập 90 ngày**, không phải benchmark ngành, dự báo conversion hay bảo đảm thống kê. Nếu cần đổi, phải đổi trước khi xem kết quả của gate, ghi old/new value, lý do, owner và ngày hiệu lực trong Decision Log; không hạ ngưỡng hồi tố để biến FAIL thành GO.

## 2. Gate 0 — Commitment, Day 7

**Câu hỏi:** chủ dự án có chấp nhận mô hình và đủ nguồn lực tối thiểu không?

### GO khi tất cả đúng

- [ ] Chấp nhận doanh thu không được bảo đảm trong 90 ngày.
- [ ] Một utility, một ICP dự kiến, một thị trường/ngôn ngữ.
- [ ] 8–12 giờ/tuần; hoặc 6–8 giờ và phạm vi `LEAN`.
- [ ] Budget setup/month/experiment có trần.
- [ ] Có ngày review cố định và người có quyền launch/rollback.
- [ ] Đồng ý duy trì kiểm tra sau Day 90.
- [ ] Bot không tự chi tiền, accept terms, KYC, tax, payout hay claim rủi ro.

### GO-LEAN

Giảm còn 8 sản phẩm, 3 decision assets, giữ automation ở L1/L2. Trust/legal pages vẫn bắt buộc và không tính vào quota. Không cắt data evidence, disclosure, tracking, QA hoặc backup.

### STOP nếu

Chỉ chấp nhận “hoàn toàn thụ động, không cần kiểm duyệt” nhưng vẫn muốn bot tự publish giá/claim/link/financial decisions.

## 3. Gate 1 — Problem–Demand Fit, Day 21

**Câu hỏi:** có vấn đề lặp lại đủ cụ thể để xây utility không?

### GO khi

- [ ] Một segment và JTBD rõ.
- [ ] ≥30 evidence records, ≥5 loại nguồn và ≥10 URL/domain.
- [ ] ≥10 bằng chứng liên quan cluster được chọn.
- [ ] Có evidence phản bác, không chỉ xác nhận.
- [ ] 40–50 query map; ≥10 query decision/comparison/cost intent.
- [ ] Audit ≥5 competitor/utilities.
- [ ] Khoảng trống cụ thể: ví dụ không tính tổng budget, thiếu trade-offs hoặc data date.
- [ ] Paper prototype được ≥3 người hiểu.

### REWORK/PIVOT

- `REWORK`: cluster quá rộng, evidence thiếu diversity, prototype wording khó hiểu.
- `PIVOT`: sau một vòng rework vẫn không có vấn đề lặp lại hoặc utility không tạo giá trị hơn content thường.

## 4. Gate 2 — Monetization Feasibility, Day 28

**Câu hỏi:** có sản phẩm, chương trình và đường nhận tiền hợp lệ không?

### GO khi

- [ ] 12–15 products (`LEAN`: 8) phù hợp ICP và có official data.
- [ ] ≥3 program candidates không có xung đột rõ với website/utility/channel; hoặc backup monetization path.
- [ ] Region/currency/purchase availability được kiểm.
- [ ] Payout method/currency/KYC/tax có đường thực hiện cho chủ dự án.
- [ ] Program restrictions, link/sub-ID, attribution, validation và termination đã ghi.
- [ ] Economics là assumptions/range, không lời hứa.
- [ ] Non-affiliate alternatives không bị loại khỏi recommendation.

### CONDITIONAL

Program application đang chờ nhưng problem/product/data tốt. Được build/launch với official non-affiliate links; không giả tracking/approval.

### NO-GO cho program cụ thể

- Country/payout/channel không hỗ trợ.
- Site/tool chưa được approved theo yêu cầu.
- Điều cấm trọng yếu chưa hiểu.
- Chỉ có thể kiếm tiền bằng self-referral/spam/cookie stuffing/misleading claim.

Nếu mọi program đều NO-GO và không có đường monetization dự phòng hợp lý, quay lại opportunity trước build.

## 5. Gate 3 — Data & Scoring, Day 35

**Câu hỏi:** dữ liệu và recommendation có đủ tin cậy để code không?

### GO khi

- [ ] 12–15 products (`LEAN`: 8).
- [ ] Dataset release manifest khóa exact schema/data/scoring/stack-rules versions, files/checksum, reviewer và rollback version.
- [ ] Critical evidence/freshness coverage = 100%; critical `UNKNOWN/STALE/CONFLICT/BLOCKED` không vào public candidate/stack.
- [ ] Noncritical freshness coverage ≥98%; phần còn lại bị ẩn hoặc gắn trạng thái rõ, không trình bày như dữ liệu mới.
- [ ] Mỗi critical field có evidence ID/URL, checked date, owner và next check; unknown không bị biến thành no/zero.
- [ ] Nếu hỗ trợ `team_size > 1`, team pricing model/unit/included-minimum seats và actual-charge rule có evidence; nếu không, input khóa ở `team_size = 1`.
- [ ] Scoring Rules có hard filters, anchors 0–5, `UNKNOWN` behavior, weights, reason codes và version; Stack Rules có Valid Stack Floor, coverage, duplicate, compatibility, cost và tie order.
- [ ] 10 Golden Tests gồm happy/boundary/no-result/missing/stale/duplicate-incompatible và team pricing nếu áp dụng.
- [ ] 100% hard constraints pass thủ công.
- [ ] ≥90% expected soft ordering được reviewer chấp nhận.
- [ ] User Fit không dùng commission.
- [ ] Component scores truy được tới rule ID + evidence IDs; Evidence Confidence tách thành completeness/freshness/source authority/consistency và không che critical blocker.
- [ ] Người mới đọc method hiểu Fit Score chỉ tương đối trong candidate set/input/versions, và giải thích được lý do top result.

### NO-GO tạm thời

Missing price/must-have/region evidence, source permission chưa rõ, hoặc scoring không tái tạo được.

## 6. Gate 4 — UX, Day 42

**Câu hỏi:** người dùng mục tiêu có hoàn thành đúng mà không bị dẫn sai không?

### GO khi

- [ ] 5 testers; ≥4/5 hoàn thành ba task không trợ giúp.
- [ ] ≥4/5 hiểu lý do recommendation và trade-off.
- [ ] Không có hiểu nhầm có thể làm mua sai.
- [ ] No-result nói thật và cho cách nới điều kiện an toàn.
- [ ] Disclosure/CTA/destination dễ hiểu trên prototype.

Nếu chỉ có 3 testers, cần 3/3 pass và gắn `LOW_SAMPLE_CONFIDENCE`; tiếp tục test sau launch.

## 7. Gate 5 — Functional MVP, Day 49

**Câu hỏi:** utility chạy đúng và có thể phục hồi không?

### GO khi

- [ ] 100% Golden hard constraints đúng.
- [ ] ≥90% soft ordering đúng rubric đã khóa.
- [ ] Cùng normalized input + exact input-schema/data/scoring/stack-rules/page versions tạo cùng stack, thứ tự, component scores và reason codes.
- [ ] `Best Fit`, `Cheapest Qualified` và `Alternative` đều qua cùng Valid Stack Floor; `Maximum Automation` bị bỏ nếu chưa có rubric/evidence/Golden Tests đầy đủ.
- [ ] Deterministic reason luôn có; nếu dùng LLM để diễn đạt, prompt/model/output được log và không thêm claim.
- [ ] Mobile/keyboard/no-result/loading/error paths pass.
- [ ] 0 P0/P1.
- [ ] Beginner Track chạy bằng static UI + versioned public JSON/manifest + direct approved links, không bắt buộc backend/database/login/router.
- [ ] Outbound CTA lookup đúng Link Registry contract bằng test fixture không thể dẫn production/affiliate destination; exact production links được duyệt ở Gate 7.
- [ ] Secret không ở source/public file.
- [ ] Có preview, test report, backup/restore và rollback target đã chạy.

Một P1 về budget, market, must-have, disclosure hoặc redirect là `NO-LAUNCH` dù giao diện đẹp.

## 8. Gate 6 — Content & Trust, Day 63

**Câu hỏi:** website có đủ giá trị gốc và niềm tin để public không?

### GO khi

- [ ] `STANDARD`: ≥5 decision assets chất lượng **gồm utility**; `LEAN`: ≥3 **gồm utility**; trust/legal pages bắt buộc tính ngoài quota.
- [ ] Methodology/Data Sources, About, Update/Editorial Policy.
- [ ] Affiliate Disclosure, Privacy, Contact và policy cần theo thị trường.
- [ ] Critical claims 100% sourced/current.
- [ ] Không fake experience/review/rating/quote.
- [ ] Không merchant-copy spin, near-duplicate hoặc trang hàng loạt chỉ để rank.
- [ ] Disclosure gần recommendation/CTA; affiliate links `rel="sponsored"`.
- [ ] Original value đưa người đọc vào utility một cách tự nhiên.

Thiếu một critical trust/compliance item là `NO-PUBLISH`, không “đăng trước sửa sau”.

## 9. Gate 7 — Launch, Day 70

**Câu hỏi:** production có an toàn, đo được và vận hành được không?

### GO khi

- [ ] G0–G6 còn hiệu lực; không source/terms/material change mới chưa review.
- [ ] 0 P0/P1; P2 có owner/acceptance.
- [ ] Production tách preview/staging.
- [ ] Data/link/disclosure/mobile/event QA pass trên live.
- [ ] Search Console verified; sitemap/crawl/index intent đúng.
- [ ] Common link controls pass: exact Link Registry version; `link_type = AFFILIATE | OFFICIAL_NON_AFFILIATE | FALLBACK` tách khỏi `delivery_mode = DIRECT | ROUTER`; allowlisted destination, inactive/invalid safe fallback, disclosure + qualifying `rel`, không auto-click/self-click/cookie stuffing và event không trùng.
- [ ] **Direct branch (mặc định):** `delivery_mode=DIRECT`; CTA dùng exact approved URL và `product_outbound_click` ghi đúng `link_type`; chỉ `link_type=AFFILIATE` được suy ra thành affiliate click.
- [ ] **Router branch (chỉ nếu bật):** `delivery_mode=ROUTER`, terms cho phép; `/go/{link_id}` chỉ lookup allowlist, không nhận arbitrary URL; expiry/domain/sub-ID/fallback/rate limit/log/secret/security test đều pass. Nếu không đạt, quay về `DIRECT` hoặc `NO-LAUNCH` cho link đó.
- [ ] Analytics không nhận PII; consent behavior đúng approved design.
- [ ] Backup restore drill pass; kill switch/incident owner có thật.
- [ ] Business/data/compliance/function owners sign off.

### SOFT-LAUNCH

Cho phép public hạn chế nếu program approval đang chờ và dùng official non-affiliate links. Không gọi là monetized launch.

## 10. Gate 8 — Safe Automation, Day 84

**Câu hỏi:** job nào đủ tin cậy để chạy theo lịch hoặc tự áp dụng low-risk changes?

Phân biệt hai loại quyền:

- **Runtime authority:** code đã publish được tính recommendation, render result, phát event và mở link đã allowlist mà không cần duyệt từng lượt.
- **Change authority:** quyền thay data/rule/link/copy/config production. Quyền này cấp theo từng task/action/risk; runtime chạy ổn không tự động cấp quyền thay đổi.

### GO-L2 khi

- [ ] Manual SOP pass ≥3 lần.
- [ ] Job có allowlist, timeout, retry, rate/budget limit, idempotency.
- [ ] Hai acceptance runs pass.
- [ ] Failure drill bắt được link/data/job errors.
- [ ] Material change luôn vào approval queue.
- [ ] Logs/alerts/owner/kill switch/rollback pass.

### GO-L3-LIMITED chỉ khi

- [ ] Một task type đảo ngược được và explicit allowlist.
- [ ] ≥20 **distinct supervised runs** của đúng task đó; retry, duplicate log hoặc chạy lại cùng input không được tính thành run mới.
- [ ] Các run trải trên ≥14 ngày **và** bao phủ ít nhất một chu kỳ freshness/SLA hoàn chỉnh của task; nếu SLA dài hơn 14 ngày thì phải chờ hết chu kỳ dài hơn.
- [ ] Case coverage có bằng chứng: no-change, valid low-risk change, duplicate/idempotency, timeout/retry, auth/rate-limit hoặc failure tương đương, source conflict, rollback và kill switch.
- [ ] 0 critical error; mọi acceptance/guardrail bắt buộc pass và QA của validation checks đạt ngưỡng đã khóa (starter: ≥99%).
- [ ] Rollback/alert/kill switch đã test.
- [ ] Không thay price, hard constraint, ranking, disclosure, terms hoặc legal claim.
- [ ] Owner ký rõ action/field/source được phép, max batch/rate/budget, expiry/review date và điều kiện tự hạ về L2.

Mặc định Day 84 là `KEEP-L2`. `KEEP-L1/L2` là PASS; `GO-L3-LIMITED` là ngoại lệ có evidence, không phải mục tiêu phải đạt hay “tự động hóa tối đa”.

## 11. Gate 9 — Quyết định Day 90

**Câu hỏi:** nên tiếp tục cùng utility, sửa hướng, chờ, pivot hay stop?

### A. Foundation gate — bắt buộc trước mọi scale

- [ ] Website/utility/tracking/reconciliation operational.
- [ ] 0 open P0/P1.
- [ ] Critical evidence/freshness coverage 100%; critical conflict/unknown/stale public = 0.
- [ ] Noncritical freshness coverage ≥98% hoặc phần còn lại ẩn/gắn trạng thái rõ.
- [ ] Recommendation reproducibility 100% trên sample audit.
- [ ] Program/channel/link active hoặc non-affiliate launch được ghi rõ.
- [ ] Exception queue xử lý được trong time budget.

Foundation fail → không scale; `REWORK/HOLD/STOP` tùy nguyên nhân.

### B. Tín hiệu sơ khởi để `ITERATE` hoặc `SCALE-CANDIDATE`

Tìm tối thiểu hai tín hiệu độc lập, ví dụ:

- ≥5 pages liên quan được index.
- Khoảng ≥100 relevant search impressions hoặc xu hướng tăng nhiều tuần.
- ≥20 qualified utility starts.
- ≥10 utility completions.
- ≥5 qualified outbound clicks.
- ≥3 phản hồi tự nhiên/cụ thể đúng ICP.
- Có một active affiliate program hoặc monetization path khả thi.

Đây là chỉ báo học tập, không chứng minh doanh thu lặp lại.

### C. Quyết định

#### `ITERATE 30 DAYS`

Chọn khi foundation pass, utility/tester có tín hiệu hữu ích nhưng discovery/conversion data còn nhỏ hoặc mới index. Giữ một ngách, chọn 1–2 experiments, đặt review date.

#### `SCALE_CURRENT — NỘI BỘ NHẸ`

Được tăng độ sâu data/content hoặc UX trong **cùng utility** khi foundation pass và có ≥2 tín hiệu sơ khởi. Không tăng budget lớn, không mở ngách mới, không auto-publish material changes.

#### `SCALE COMMERCIAL / MỞ NGÁCH THỨ HAI`

Không chỉ dựa vào Day-90 indicators. Cần thêm:

- Ít nhất một `Final Commission`; tốt hơn là nhiều final conversions qua hơn một cohort.
- Final EPC/RPV đủ mature và contribution có xu hướng dương sau chi phí.
- Payout path đã được xác nhận hoặc ít nhất final due không có blocker KYC/payout.
- Conversion không đến từ internal/self/incentivized/vi phạm terms.
- Vận hành ổn định ≥4 tuần, không compliance incident.
- Capacity giữ freshness/trust cho asset hiện tại sau khi mở rộng.

Nếu mới có một final commission, trạng thái đúng thường là `SCALE-CANDIDATE`, không “nhân 10 lần”.

#### `PIVOT`

Chọn khi sau một rework cycle:

- Evidence nhu cầu không lặp lại/ICP không dùng utility.
- Data đủ nhưng recommendation không hữu ích.
- Sản phẩm/program/channel/payout không phù hợp.
- Original value không khác thin affiliate content.
- Maintenance vượt time/budget một cách cấu trúc.

Quay lại gate thấp nhất bị sai; giữ logs/evidence để không lặp lại.

#### `HOLD`

Chọn khi blocker ngoài hệ thống có ngày/owner: program review, index delay, vendor clarification. Giữ site an toàn, dừng job/chi phí không cần; đặt ngày đánh giá lại.

#### `STOP`

Chọn khi tiếp tục chỉ có thể bằng cách vi phạm terms/quyền/privacy, gây hiểu nhầm, làm giả evidence/traffic/review, hoặc chi phí/rủi ro vượt giới hạn được chấp nhận.

## 12. Kill / Maintain / Scale sau Day 90

### `TEST`

Chưa đủ matured data. Có hypothesis, timebox, budget và kill conditions.

### `WATCH`

Có discovery/usage nhưng funnel hoặc data maturity chưa rõ. Không tăng resource; sửa blocker lớn nhất.

### `MAINTAIN`

Utility hữu ích, data/trust khỏe, contribution chưa đủ mạnh để scale. Giữ freshness, link, tracking và content core.

### `SCALE-CANDIDATE`

Foundation khỏe + repeatable signals + economics có triển vọng. Chuẩn bị experiment nhỏ; chưa mặc định cấp budget.

### `SCALE`

Chỉ khi evidence commercial matured, positive contribution, low compliance risk và operations capacity. Tăng theo bước 10–25%, đặt guardrail/rollback; đây là gợi ý vận hành, không benchmark lợi nhuận.

### `KILL`

Kill ngay vì risk hoặc kill sau test vì không khả thi:

- Immediate: terms/privacy/security/fraud/misleading/unsafe link.
- Evidence-based: đủ discovery/usage sample theo pre-set test nhưng utility không giải quyết pain sau ≥2 meaningful iterations.
- Structural: data không thể sử dụng hợp pháp/tin cậy; payout không thể nhận; cost duy trì luôn lớn hơn plausible contribution.

Không kill chỉ vì chưa có commission khi trang mới index hoặc sample chưa mature.

## 13. Decision memo bắt buộc

```text
Gate/Asset:
Ngày và reviewer:
Decision:

FACTS:
INFERENCES:
UNKNOWNS:
DATA WINDOW/MATURITY:
GUARDRAILS:
OPTIONS CONSIDERED:
DECISION + WHY:
BUDGET/SCOPE IMPACT:
OWNER/NEXT ACTION:
REVIEW DATE:
ROLLBACK/KILL CONDITION:
```

