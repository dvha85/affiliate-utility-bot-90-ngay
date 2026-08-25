# 03 — Lộ trình chi tiết Day 1–90

## Quy ước trước khi bắt đầu

Mỗi ngày có bốn phần: **Học → Làm → Lưu đầu ra → Kiểm điều kiện đạt**. Không đánh dấu hoàn thành nếu chỉ đọc hoặc hỏi AI.

Nhịp chuẩn là 8–12 giờ/tuần. Nếu chỉ có 6–8 giờ, dùng phạm vi `LEAN`: 8 sản phẩm, 3 decision assets cùng các trust/legal pages bắt buộc, và 3 người test; không cắt tracking, nguồn dữ liệu, disclosure, QA hay backup.

Các con số trong lộ trình là **starter thresholds nội bộ để quản lý bài tập**, không phải benchmark ngành hay dự báo doanh thu. Chỉ thay ngưỡng trước khi xem kết quả, ghi lý do/owner/ngày hiệu lực trong Decision Log; không hạ ngưỡng sau khi thấy dữ liệu xấu.

### Beginner Track mặc định và phân vai

- **Bạn/chủ dự án:** ra quyết định ICP/phạm vi/ngân sách, kiểm nguồn, duyệt data/rule/link/copy/terms, tạo tài khoản cần thiết và bấm publish.
- **Coding agent:** nhận Work Packet, dựng preview tĩnh từ CSV/JSON có version, triển khai rule deterministic, chạy test và giao bằng chứng. Agent không tự duyệt nguồn, terms, price, ranking hoặc link production.
- **Research/content agent:** tạo candidate/draft vào staging và gắn nguồn; không tự publish.
- **Runtime:** chỉ tính toán/hiển thị từ exact versions đã publish. Quyền runtime không đồng nghĩa quyền thay đổi production.

Mặc định Day 1–90 là website tĩnh + public JSON được build từ CSV đã duyệt + direct approved product/affiliate links. Không cần biết code, database, backend, login hay router để hoàn thành. Router chỉ là nhánh tùy chọn khi có nhu cầu đo lường rõ, terms cho phép và người có năng lực security-review.

Mỗi buổi:

- [ ] 15–25 phút học đúng mục được nêu.
- [ ] 45–120 phút tạo đầu ra.
- [ ] 10–20 phút tự kiểm và cập nhật worklog.
- [ ] Ghi blocker cụ thể; không dùng “bận” hoặc “khó” như chẩn đoán.
- [ ] Chỉ chuyển ngày khi điều kiện đạt, hoặc ghi rõ `CARRY_OVER` sang ngày đệm.

## Tuần 1 — Hiểu mô hình và khóa phạm vi

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 1 | Bot là workflow + data + website; tiền đến từ giá trị, traffic và conversion | Điền mục tiêu, giới hạn, lịch, ngân sách; chọn tạm một thị trường/ngôn ngữ | [Project Charter](templates/01-project-charter.md) v1 | Một utility, một nhóm người dùng, một ngôn ngữ/thị trường; ngân sách và số giờ có trần |
| 2 | Funnel doanh thu và các trạng thái tiền | Vẽ `impression → visit → utility → click → order → valid → final → paid` | Funnel Map | Giải thích được vì sao click, pending order và payment khác nhau |
| 3 | Tỷ lệ, mẫu số và expected value | Tạo ba kịch bản thận trọng/cơ sở/tốt; đánh dấu từng con số là fact hay assumption | Revenue Assumption Sheet | Không có giả định nào được trình bày như dữ kiện; công thức tính đúng |
| 4 | Niềm tin, disclosure, privacy và rủi ro AI | Viết red lines và kill conditions | Red-line Policy | Có: không review giả, không bịa, không spam, không thu thập trái phép, không auto-spend/auto-accept terms |
| 5 | Quản lý dự án nhỏ | Tạo board `Backlog/Doing/Review/Done`, Evidence Library và Decision Log | Project Board + thư mục bằng chứng | Mỗi đầu ra có owner, trạng thái, ngày và nơi lưu |
| 6 | Chọn công cụ theo nhu cầu | Chốt `Beginner Track`: versioned CSV/JSON → static UI → direct approved link; chỉ ghi deviation nếu thật sự cần backend/router | Stack Decision Draft | Người không biết code vẫn kiểm được input/output/test; có phương án dự phòng, budget; chưa mua khi chưa duyệt |
| 7 | Review có bằng chứng | Tự kiểm M0–M1, rà Charter và chạy Gate 0 | Weekly Review 01 | Gate 0 `GO` hoặc `GO-LEAN`; mọi fail có kế hoạch sửa tối đa 7 ngày |

### Checklist kết tuần 1

- [ ] Tôi nói được utility giúp ai làm gì trong một câu.
- [ ] Tôi không kỳ vọng bot tự xử lý KYC, tiền, thuế hay điều khoản.
- [ ] Tôi đã chọn lịch review cố định hằng tuần.
- [ ] Tôi biết chi phí nào đang bằng 0/chưa biết/đã duyệt.

## Tuần 2 — Chọn đúng người dùng và vấn đề

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 8 | Segment khác mô tả nhân khẩu học | Liệt kê 5–7 nhóm: creator mới, solo affiliate, freelancer, solo business… | Segment Map | Mỗi nhóm có tình huống, mục tiêu, kỹ năng, ngân sách và khó khăn riêng |
| 9 | Job-to-be-Done | Chọn hai nhóm ứng viên; viết tình huống → động lực → kết quả | Hai JTBD Draft | Câu không mô tả tính năng của bot mà mô tả việc người dùng cần hoàn thành |
| 10 | Bằng chứng mạnh/yếu | Thu 15 phát biểu/tình huống đầu từ nhiều loại nguồn | [Research Log](templates/02-research-log.csv) batch 1 | Mỗi dòng có tóm tắt, URL, ngày, segment, intent và confidence |
| 11 | Quan sát trung lập | Hỏi 3–5 người bằng câu hỏi không chào bán; nếu không thể, bổ sung quan sát công khai để tổng Research Log đạt 30 records, ≥5 loại nguồn và ≥10 URL/domain | Interview/Observation Notes + Research Log đủ mẫu | Có cả bằng chứng ủng hộ và phản bác; không hỏi “bạn có mua không?” |
| 12 | Tần suất, mức đau, hành vi hiện tại | Gom evidence thành 5–7 pain clusters | Pain Map | Mỗi cluster có số evidence và cách người dùng đang giải quyết |
| 13 | Chấm ưu tiên vấn đề | Chấm tần suất, độ đau, decision intent, utility fit, data access, monetization, risk | [Opportunity Card](templates/03-opportunity-card.md) | Công thức và nguồn điểm rõ; không chọn chỉ vì commission cao |
| 14 | Thu hẹp ICP | Chọn một segment + một JTBD; viết problem statement và non-goals | Audience Brief v1 | Người lạ hiểu ai, trong hoàn cảnh nào, cần quyết định gì và utility không làm gì |

### Checklist kết tuần 2

- [ ] ICP không còn là “mọi người dùng AI”.
- [ ] Có ít nhất ba pain cluster lặp lại.
- [ ] Có bằng chứng phản bác được giữ lại.
- [ ] Chưa xây website trước khi vấn đề được khóa.

## Tuần 3 — Kiểm chứng nhu cầu và khoảng trống

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 15 | Intent: học, so sánh, tính toán, chọn, mua | Phân loại 20 query mẫu | Intent Exercise | Nêu được việc người tìm muốn hoàn thành, không chỉ nhắc từ khóa |
| 16 | Seed query từ JTBD | Viết query theo `best`, `vs`, `alternative`, `pricing`, `for`, `under budget`, `stack` | 30 Seed Queries | Có intent thông tin, so sánh, chi phí và quyết định |
| 17 | Topic/problem cluster | Mở rộng và nhóm 40–50 query | Query Map v1 | Mỗi cụm trỏ tới một câu hỏi utility hoặc một page intent |
| 18 | Audit kết quả hiện hữu | Quan sát 10 query quan trọng; ghi loại kết quả, điểm mạnh/yếu, ngày | SERP Log | Không scrape tự động Google; ghi đây là snapshot, không phải dữ liệu vĩnh viễn |
| 19 | Audit utility/đối thủ | Trải nghiệm tối thiểu 5 công cụ hoặc website so sánh | Competitor Matrix | So sánh input, output, explanation, freshness, disclosure, CTA và mobile |
| 20 | Value proposition + paper prototype | Vẽ Stack Builder trên giấy; cho 3 người làm ba task không chào bán | Paper Test Notes | Người test hiểu câu hỏi, output và lý do mà không được hướng dẫn giữa chừng |
| 21 | Tổng hợp | Viết Opportunity Brief và chạy Gate 1 | Opportunity Brief | Có 30 evidence, ≥10 tín hiệu lặp, khoảng trống cụ thể và paper test đủ hiểu |

### Nếu Gate 1 không đạt

- `REWORK`: dành tối đa 7 ngày bổ sung nguồn/thu hẹp segment/chỉnh problem.
- `PIVOT`: đổi segment hoặc JTBD, quay lại Day 8; giữ Evidence Log cũ.
- Không “giải quyết” thiếu nhu cầu bằng cách viết nhiều bài hơn.

## Tuần 4 — Kiểm chứng sản phẩm và khả năng kiếm tiền

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 22 | Commission, attribution, cookie window, reversal, payout | Vẽ vòng đời giao dịch và nguồn sự thật cho mỗi trạng thái | Affiliate Lifecycle | Hiểu commission rate cao không đồng nghĩa expected payout cao |
| 23 | Product universe | Liệt kê khoảng 30 AI/SaaS tool liên quan đúng ICP/JTBD | Product Longlist | Mỗi sản phẩm map tới ít nhất một need; chưa chấm theo commission |
| 24 | Đọc affiliate terms | Tìm chương trình chính thức; đọc eligibility, channel rules, trademark, self-referral, payout, data use | 3–5 [Program Reviews](templates/04-affiliate-program-review.md) | Mỗi claim có official URL + checked date; điểm chưa rõ là `BLOCKED`, không tự suy diễn |
| 25 | Market fit | Kiểm tra region, language, currency, payment, plan availability | Market Fit Sheet | Không đưa sản phẩm không thể mua/dùng trong target market vào eligible set |
| 26 | Unit economics v0 | Cập nhật mô hình với price/commission có nguồn; tách fact/assumption | Economics v1 | Có downside case, cost trần và không gọi pending là revenue chắc chắn |
| 27 | Chọn danh mục MVP | Rút còn 12–15 sản phẩm (`LEAN`: 8), 3–5 category, nhiều mức giá; giữ lựa chọn non-affiliate | MVP Product List | User fit không phụ thuộc có chương trình; không một vendor nào chiếm toàn bộ category |
| 28 | Rủi ro phụ thuộc | Chọn đường kiếm tiền chính, backup và phương án launch khi đang chờ duyệt | Monetization Decision | Gate 2 `GO/CONDITIONAL`; con người tự duyệt terms/KYC/tax/payout |

### Điều kiện `CONDITIONAL`

Được xây tiếp nếu product/problem tốt nhưng affiliate approval còn chờ. Dùng official product URL, không giả affiliate tracking. Không được dùng link affiliate của người khác hoặc tự tạo mã tracking.

## Tuần 5 — Xây dữ liệu và phương pháp gợi ý

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 29 | Schema, data type, null và provenance | Định nghĩa product/plan/feature/audience/source/link fields và ID/version contract | Data Dictionary | Có source URL, checked date, region, currency, billing interval, confidence, status; nếu `team_size > 1` có pricing model/unit/included-min seats hoặc khóa MVP ở team_size=1 |
| 30 | Trích xuất dữ kiện có nguồn | Nhập 6 sản phẩm đầu từ nguồn chính thức; dùng 4–6 critical features cho mỗi sản phẩm | Dataset Batch 1 | Mọi giá/feature/term quan trọng có source evidence |
| 31 | Missing data và consistency | Nhập phần còn lại, ghi `UNKNOWN` thay vì 0/false | Dataset v1 | 12–15 sản phẩm (`LEAN`: 8), không có ô mơ hồ cho field bắt buộc |
| 32 | Normalization | Chuẩn hóa monthly/annual, VAT/tax assumption, currency, limits, trial | Normalization Rules | So sánh cùng region/currency/billing assumption; MVP loại/warn currency không so được; không gọi annual equivalent là monthly billed |
| 33 | Hard filter trước weighted score | Viết eligibility filters, component anchors 0–5, reason codes và User Fit trong [Scoring Rules](templates/24-scoring-rules.csv); viết Valid Stack Floor/duplicate/compatibility/output selection trong [Stack Rules](templates/29-stack-rules.md) | Scoring + Stack Spec v1 | Budget/region/must-have/evidence block không bị điểm mềm ghi đè; `UNKNOWN` không bị chấm 0; commission không vào User Fit/tie-break |
| 34 | Golden tests | Tạo 10 persona: happy, boundary, no-result, missing/stale, duplicate/incompatible và team pricing nếu hỗ trợ | [Golden Test Set](templates/05-golden-tests.csv) | Mỗi row có input, expected valid stacks/rejection/output/reason trước khi chạy engine; `Maximum Automation` chỉ có test nếu rubric/evidence đủ |
| 35 | Manual validation và release | Tính tay 10 test, lưu [Component Scores](templates/26-recommendation-component-scores.csv), khóa exact data/scoring/stack versions vào [Dataset Manifest](templates/25-dataset-release-manifest.csv) | Data & Scoring Approval | 100% hard constraints; ≥90% soft ordering theo rubric; critical freshness 100%, noncritical ≥98%; checksum/rollback/reviewer có đủ; Gate 3 đạt |

### User Fit Score mặc định

| Thành phần | Trọng số |
|---|---:|
| Must-have need coverage | 30% |
| Budget fit | 20% |
| Workflow/team fit | 15% |
| Value for cost | 15% |
| Ease of adoption | 10% |
| Integration fit | 5% |
| Use-case risk/limitations | 5% |

Mỗi thành phần chấm 0–5. Nếu hard filter fail thì loại, không tính score. Evidence Confidence hiển thị riêng. Commercial Score chỉ dùng phân tích kinh tế/vận hành nội bộ; không chọn, sắp xếp, làm nổi bật hay phá hòa recommendation công khai.

Fit Score là so sánh tương đối trong candidate set, user input và exact data/scoring/stack-rules versions của lần chạy; không phải xác suất mua thành công, chất lượng tuyệt đối hay lời bảo đảm.

## Tuần 6 — UX và prototype

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 36 | User journey | Vẽ landing → questions → result → compare → click → feedback | User Flow | Hoàn thành không cần tài khoản/email; có no-result/error route |
| 37 | Hỏi ít nhưng đủ | Viết 5–8 câu hỏi, gồm budget và khả năng trả trước theo năm; nêu rule nào dùng câu trả lời | Question Set v1 | Mỗi câu ảnh hưởng filter/score/explanation; monthly equivalent không vượt qua giới hạn upfront; bỏ câu hỏi “để sau có thể cần” |
| 38 | Result hierarchy | Thiết kế stack, total cost, score, why, limitations, alternative, checked date | Result Spec | Phân biệt fact/assumption; data stale/unknown hiện rõ |
| 39 | UX writing và CTA trung thực | Viết help text, validation, no-result, disclosure và CTA | Copy Deck | Không urgency giả, không che đích đến, disclosure gần CTA |
| 40 | Clickable prototype | Tạo wireframe/prototype happy path + hai no-result/error cases | Prototype v1 | Đi hết luồng, dùng được trên màn hình nhỏ ở mức prototype |
| 41 | Usability test | Quan sát 5 người × 3 task; không giải thích giữa chừng | Usability Notes | Ghi task success, thời gian, lỗi, lời nói; không chỉ hỏi “có thích không?” |
| 42 | Sửa và duyệt | Sửa câu hỏi/flow/result; chạy lại task chính | Prototype v2 | ≥4/5 hoàn thành và hiểu lý do; không có hiểu nhầm gây mua sai; Gate 4 đạt |

### Khi không tìm được 5 người test

Dùng 3 người và yêu cầu 3/3 hoàn thành; ghi `LOW_SAMPLE_CONFIDENCE`. Không dùng chính bạn làm đủ cả ba người. Đây là nghiên cứu sản phẩm, không phải mời chào mua bot.

## Tuần 7 — Xây MVP

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 43 | Work Packet và skeleton | Bạn khóa input/output/acceptance; cung cấp Link Registry schema + test fixture; coding agent tạo preview, landing và bước build CSV → public JSON + manifest | MVP Skeleton | Exact core data/rule versions đã duyệt; fixture ghi rõ TEST, không dẫn link production; build không sửa source data; có backup; secret không public |
| 44 | Form và validation | Xây câu hỏi, lựa chọn, default an toàn và lỗi nhập | Working Questionnaire | Không gửi khi thiếu bắt buộc; keyboard/mobile dùng được; không hỏi PII không cần thiết |
| 45 | Rule engine | Coding agent triển khai hard filters → component rules → valid-stack rules từ exact versions | Recommendation Engine v1 | Golden tests hard constraints đều đúng; cùng normalized input + exact versions cho cùng output, component score và reason code |
| 46 | Màn hình kết quả | Hiển thị stack, actual/upfront/monthly-equivalent total, deterministic why, limitations, Confidence và checked/version date | Result Page v1 | User phân biệt dữ kiện/giả định; hiểu Fit Score là tương đối; giá ghi đúng kỳ thanh toán; LLM không thêm claim |
| 47 | Compare và alternatives | Thêm `Best Fit`, `Cheapest Qualified`, `Alternative`; chỉ thêm `Maximum Automation` khi rubric/evidence/tests đạt; xử lý no-result | Comparison Flow | Mọi output qua cùng Valid Stack Floor; alternative có trade-off thật; thiếu rubric thì không render Maximum Automation |
| 48 | Resilience và mobile | Xử lý loading, data error, link thiếu, stale, timeout | Resilience Report | Không màn hình trắng; lỗi có hành động tiếp; hai kích thước mobile pass |
| 49 | Functional QA | Chạy full Golden Test Set + exploratory tests; audit component score/rule/evidence/manifest; phân loại lỗi P0–P3 | MVP v1 | 100% hard constraints, ≥90% soft ordering theo rubric, release versions tái lập được, 0 P0/P1; Gate 5 đạt |

### Severity khi test

- `P0`: gây hại/mất dữ liệu, lộ secret, redirect sai/nguy hiểm — dừng ngay.
- `P1`: utility không hoàn thành, budget/hard constraint sai, disclosure/link sai — cấm launch.
- `P2`: một tính năng phụ sai nhưng có workaround — sửa trước hoặc ghi acceptance rõ.
- `P3`: lỗi trình bày nhỏ — đưa backlog.

## Tuần 8 — Tracking, quyền riêng tư và nền kỹ thuật

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 50 | Event contract và KPI | Định nghĩa event, trigger, params, owner, source và business question | Measurement Plan | Có `utility_start`, `utility_complete`, `result_view`, `product_outbound_click` + `link_type` + `delivery_mode`; chỉ derive `affiliate_click` khi `link_type=AFFILIATE`; không PII |
| 51 | Web analytics | Gắn events ở preview; bật/cấu hình consent theo thị trường sau review | Analytics v1 | Mỗi hành động test phát đúng một event; Debug/Realtime thấy đúng params |
| 52 | Link delivery | Hoàn thiện và duyệt production Link Registry thay test fixture; chọn `delivery_mode=DIRECT` cho Beginner/LEAN hoặc `ROUTER` qua `/go/{link_id}` khi program cho phép và router đã security-review | Link Delivery v1 | Exact Link Registry version được khóa; `link_type` tách khỏi `delivery_mode`; inactive không dẫn affiliate destination; qualifying `rel` đúng; không auto/self-click/open redirect |
| 53 | Dashboard funnel | Tạo bảng Search → on-site → network → commission → payment | KPI Dashboard v1 | Ordered/Valid/Final/Paid là bốn số riêng; mỗi số có source/maturity |
| 54 | Privacy và security | Inventory dữ liệu/tags/processors; MFA, least privilege, backup, retention | Security & Privacy Checklist | Không email/tên/IP raw trong sub-ID; secrets được tách; luật áp dụng được đánh dấu cần review |
| 55 | Technical SEO | Title/description/canonical/sitemap/robots/noindex/internal links | Technical SEO Checklist | Production cần index không bị chặn; staging noindex/access-controlled |
| 56 | Restore + end-to-end QA | Restore một bản test; chạy browser → result → event/log theo `delivery_mode` → dashboard | Pre-content QA | Restore thành công; event không trùng; link type/delivery/destination/limitation có bằng chứng; release rollback được; 0 P0/P1 |

### Work Packet Day 43–56 cho coding agent

Bạn không cần mô tả framework hay viết code. Giao một packet có thể kiểm bằng đầu ra:

**Input đã được bạn duyệt**

- Exact versions của Products, Plans, Feature Evidence, Source Registry; Link Registry schema + test fixture có nhãn `TEST` cho Gate 5. Exact production Link Registry do bạn duyệt ở Day 52 trước Gate 7.
- [Scoring Rules](templates/24-scoring-rules.csv), [Stack Rules](templates/29-stack-rules.md), [Dataset Manifest](templates/25-dataset-release-manifest.csv) và [Golden Tests](templates/05-golden-tests.csv).
- Question/Result/Copy specs, disclosure, event contract và `delivery_mode=DIRECT` mặc định; nếu yêu cầu `ROUTER` phải kèm terms evidence + security acceptance riêng.

**Coding agent phải giao**

- Preview URL của static UI; deterministic build/export tạo public JSON + manifest từ input đã duyệt.
- Form validation, engine, component-score audit, canonical outputs/no-result, deterministic reasons và link allowlist.
- Test report gồm Golden Tests, mobile/keyboard, event-once, stale/conflict, invalid link, error/fallback, preview-to-production và restore/rollback.
- Release note ghi exact code/data/schema/scoring/stack/link versions; không tự đổi nguồn, giá, rule, wording pháp lý hoặc production approval.

**Acceptance do bạn kiểm trước Gate 5/7**

- [ ] Cùng normalized input + exact versions luôn cho cùng stacks, thứ tự, score và reason codes.
- [ ] Website chạy mà không cần backend/database/login/router; nếu router được bật, nhánh direct vẫn là fallback an toàn đã duyệt.
- [ ] 100% hard/valid-stack tests pass; soft ordering đạt ngưỡng đã khóa; mỗi component truy được rule ID + evidence IDs.
- [ ] Reason deterministic luôn có; nếu bật LLM thì prompt/model/template/input/output được audit, không đổi fact/order và có deterministic fallback.
- [ ] `Best Fit`, `Cheapest Qualified` và `Alternative` đều qua Valid Stack Floor; `Maximum Automation` bị ẩn nếu chưa có rubric/evidence/tests đầy đủ.
- [ ] Manifest/version/data date/assumptions/limitations/disclosure hiển thị hoặc truy được; critical freshness 100%, noncritical ≥98%.
- [ ] **Gate 5:** CTA lookup đúng Link Registry test fixture, fixture không thể dẫn production/affiliate destination.
- [ ] **Gate 7:** mọi CTA tra được trong exact approved production Link Registry, có `link_type`, `delivery_mode`, destination allowlist và fallback; không secret/PII trong public file, URL hay event.
- [ ] Mỗi hành động test phát đúng một event; internal/test được gắn cờ; direct và router branches (nếu có) đều có bằng chứng.
- [ ] Mobile/keyboard/error path pass, backup restore và rollback về release trước đã chạy thật.

### Lưu ý attribution

GA4/outbound event chỉ chứng minh hành động trên site; nó không chứng minh order hay commission. Affiliate dashboard/API/export là nguồn sự thật cho trạng thái giao dịch. Chỉ truyền `subid/clickref` khi program cho phép và không bao giờ đưa PII vào đó.

## Tuần 9 — Nội dung hỗ trợ quyết định

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 57 | Content map và page intent | Ánh xạ 5 decision assets (`LEAN`: 3) vào query cluster, pain, funnel và utility entry; trust/legal pages tính riêng | Content Map | Mỗi trang có một job riêng, không phải biến thể từ khóa hàng loạt |
| 58 | Who/How/Why và trust | Viết Methodology, Data Sources, About, Editorial/Update Policy | Trust Pages | Nói đúng cách AI/automation được dùng; không giả trải nghiệm/chuyên môn |
| 59 | Decision guide | Viết hướng dẫn chọn stack cho ICP chính | Decision Guide 1 | Có decision framework, trade-off, ví dụ có giả định và đường vào utility |
| 60 | Comparison | Viết A vs B hoặc category comparison dựa trên cùng tiêu chí | Comparison Page 1 | Có use case phù hợp/không phù hợp, source/date, không copy merchant copy |
| 61 | Budget/alternative | Viết stack dưới ngân sách hoặc alternatives | Budget Page | Total cost, region/currency/billing assumptions rõ; không dùng giá stale |
| 62 | Supporting assets | Viết đủ quota 5 decision assets (`LEAN`: 3) từ Research Log; giảm số trang trước khi giảm chất lượng | Supporting Pages | Trả lời evidence thật, thêm utility/data độc đáo, không kéo dài vô ích |
| 63 | Editorial QA | Fact-check, originality, mobile, link, disclosure, rel, tracking | Launch Content Set | Standard ≥5 hoặc LEAN ≥3 decision assets, cộng trust/legal pages; critical claims 100% evidence; Gate 6 |

### Mẫu disclosure gần CTA

> Tôi có thể nhận hoa hồng nếu bạn mua qua một số liên kết trên trang này.

Ngay cạnh một link có thể thêm câu “Tôi có thể nhận hoa hồng nếu bạn mua qua liên kết này.” Đây là bổ sung, không thay disclosure đầy đủ ở gần recommendation. Disclosure phải cùng ngôn ngữ trang, rõ trên mobile và nằm trước/gần recommendation/link đầu tiên; không chỉ đặt ở footer. Thuộc tính qualifying cho công cụ tìm kiếm là việc riêng, không thay disclosure cho con người.

## Tuần 10 — Chuẩn bị và ra mắt

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 64 | Application readiness | Chuẩn bị/nộp program application đúng sự thật | Application Log | Con người tự chấp nhận terms/KYC/tax/payout; khai đúng domain/channel |
| 65 | Fact và link QA | Re-check critical data, destination, `link_id/link_version`, offer reference nếu có và disclosure | Fact/Link Report | 100% critical link/source hoạt động; không tạo click network trái phép để test |
| 66 | Device/accessibility/performance | Test desktop/mobile/keyboard/contrast/slow/error path | Cross-device Report | Không lỗi cản utility; CTA/disclosure đọc được ở viewport nhỏ |
| 67 | Soft publish | Đưa production ở chế độ hạn chế; submit sitemap/URL khi sẵn sàng | Soft-live Site | Production tách preview; sitemap fetch được; biết submit không bảo đảm index |
| 68 | Live tracking | Chạy test traffic có cờ `internal/test`; kiểm custom click event và `delivery_mode` đã chọn | Live Analytics Proof | Events đúng một lần; test excluded/labeled; không self-purchase hoặc tạo network click khi program không cho test |
| 69 | Real-user usability | 3–5 người mục tiêu làm task; thu phản hồi nghiên cứu | Launch Feedback Log | Không blocker/misleading; issue có severity/owner; không chào bán bot |
| 70 | Sửa, backup, launch | Sửa P0/P1/P2 được chấp nhận; chụp baseline; chạy Gate 7 | Launch v1 | Compliance + data + functional + tracking + rollback đều GO |

### Việc không được làm để “kích hoạt dữ liệu”

- Không tự click lặp affiliate link.
- Không tự mua qua link của chính mình nếu terms cấm hoặc chưa rõ.
- Không tạo traffic bot, incentivized click, cookie stuffing hay redirect ẩn.
- Không thêm tham số tùy ý vào affiliate URL; chỉ dùng sub-ID đúng format được phép.

## Tuần 11 — Đọc tín hiệu sớm và tối ưu

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 71 | Baseline khác target | Chụp traffic/index/funnel/click/error/cost baseline | Baseline Snapshot | Ghi data window, timezone, internal filter, sample size và maturity |
| 72 | Crawl/index/discovery | Dùng Search Console kiểm property, sitemap, page indexing và URL Inspection | Discovery Diagnostic | Tách “chưa được thấy” khỏi “không có nhu cầu”; không coi average position là doanh thu |
| 73 | Snippet và internal links | Sửa tối đa 3 trang dựa trên intent/evidence | SEO Change Log | Một lý do/thay đổi/trang; có review date; không keyword stuffing |
| 74 | Completion funnel | Tìm bước rơi lớn nhất; chạy một UX change | Experiment 01 | Một primary metric + guardrail; không sửa nhiều biến khó tách |
| 75 | Recommendation quality | Xem feedback, no-result, outlier; đề xuất rule changes | Scoring Review 01 | Mọi rule change chạy lại toàn bộ Golden Tests và cần duyệt |
| 76 | CTA quality | Làm rõ value/trade-off/destination, không tăng áp lực giả | CTA Experiment | Disclosure/rel/tracking vẫn pass; guardrail complaint/misclick không xấu |
| 77 | Weekly decision | Phân tích cohort đủ tuổi; keep/reject/inconclusive | Experiment Report 01 | Mẫu nhỏ ghi `NOT_ENOUGH_DATA`; không chọn số đẹp để kết luận |

## Tuần 12 — Vận hành bán tự động có kiểm soát

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 78 | Chỉ tự động hóa quy trình đã hiểu | Vẽ manual workflow data/link/report, failures và approvals | Automation Map | Mỗi bước có input/output/owner/error/rollback; chưa auto-publish |
| 79 | Link monitor | Tạo job kiểm tra destination/status/redirect theo phương thức được phép | Link Monitor | Bắt bộ lỗi giả lập; chỉ alert/deactivate an toàn, không tạo affiliate click giả |
| 80 | Freshness monitor | Tạo check `fresh/stale/conflict` và change queue | Change Queue | Không auto đổi price/feature/ranking; giữ before/after/source |
| 81 | AI-assisted update | `detect → draft → evidence → diff → human approve → publish` | Editorial Approval Queue | Draft luôn kèm sources; claim/trải nghiệm/material change không tự publish |
| 82 | Weekly KPI report | Tự tổng hợp discovery, funnel, data, jobs, exceptions, money states | Weekly Report v1 | Số đối chiếu được; Ordered/Final/Paid tách; missing/maturity được chú thích |
| 83 | Failure/rollback/kill switch | Giả lập source lỗi, scoring sai, job fail, secret exposure | Incident Drill | Kill switch, alert, evidence preservation, rollback và owner đều hoạt động |
| 84 | Acceptance run | Tổng hợp run log/case coverage và chạy failure + full-refresh có duyệt | Automation Acceptance | Mặc định `KEEP-L2`; chỉ đề xuất `GO-L3-LIMITED` khi có ≥20 distinct supervised runs trên ≥14 ngày, ít nhất một chu kỳ SLA/freshness hoàn chỉnh và đủ case coverage; Gate 8 |

Nếu chỉ bắt đầu thu run evidence ở Day 78 thì không thể đủ 14 ngày vào Day 84; chọn `KEEP-L2` là kết quả đúng. Chỉ dùng evidence bắt đầu sớm hơn khi đúng cùng task/version/SOP và vẫn bao phủ trọn một SLA cycle; không chạy dồn hoặc nhân retries để đủ số.

## Sáu ngày cuối — Audit và quyết định

| Ngày | Học | Làm | Đầu ra | Điều kiện đạt |
|---:|---|---|---|---|
| 85 | Data audit | Rà product/source/link/freshness/conflicts và khóa release manifest | Dataset v2 | Critical freshness/evidence coverage = 100%; noncritical freshness ≥98% hoặc phần còn lại ẩn/gắn trạng thái; critical conflict/unknown public = 0 |
| 86 | KPI theo tầng | So baseline/trend cho delivery, trust, discovery, funnel, money, operations | 90-Day KPI Draft | Không trộn sessions/clicks/order states; sample/maturity hiển thị |
| 87 | Partner readiness | Recheck approval, terms version, link status, backup program | Partner Status | Ít nhất một monetization path khả thi hoặc unblock action + owner/date |
| 88 | Economics với data thật | Thay assumptions bằng observed data; tính range cho phần chưa biết | Economics v2 | Pending không thành revenue; time/cost/fee/FX được ghi; chưa đủ data được nói rõ |
| 89 | Backlog 30 ngày | Ưu tiên impact × confidence ÷ effort/risk | Next-30-Day Plan | Sửa trust/data/funnel trước mở ngách; tối đa 1–2 experiments đồng thời |
| 90 | Gate cuối + retrospective | Chọn `SCALE_CURRENT / ITERATE / PIVOT / HOLD / STOP` | 90-Day Decision Record | Có evidence, điều chưa biết, owner, budget, review date; Gate 9 được ký |

## Bản đồ các cổng

| Gate | Ngày | Chủ đề | Kết quả hợp lệ |
|---|---:|---|---|
| G0 | 7 | Cam kết/phạm vi | GO, GO-LEAN, REWORK, STOP |
| G1 | 21 | Problem–Demand | GO, REWORK, PIVOT |
| G2 | 28 | Monetization feasibility | GO, CONDITIONAL, REWORK, NO-GO |
| G3 | 35 | Data & scoring | GO, REWORK |
| G4 | 42 | UX | GO, REWORK |
| G5 | 49 | Functional MVP | GO, REWORK, NO-LAUNCH |
| G6 | 63 | Content & trust | GO, REWORK, NO-PUBLISH |
| G7 | 70 | Launch | GO, SOFT-LAUNCH, NO-LAUNCH |
| G8 | 84 | Safe automation | KEEP-L2 (mặc định), KEEP-L1, GO-L2, GO-L3-LIMITED |
| G9 | 90 | Portfolio decision | SCALE_CURRENT, ITERATE, PIVOT, HOLD, STOP |

Chi tiết điều kiện và cách quyết định nằm trong [Cổng Go/No-Go](08-go-no-go.md). Không tự thay ngưỡng sau khi đã nhìn kết quả mà không ghi Decision Log.

## Khi bị trễ lịch

Ưu tiên giữ theo thứ tự:

1. Nguồn dữ liệu, terms và privacy/compliance.
2. Hard constraints và Golden Tests.
3. Tracking và backup/rollback.
4. Utility core.
5. Trust/legal pages và decision assets theo phạm vi đã khóa (`Standard`: 5; `LEAN`: 3).
6. Automation.

Cắt từ Standard xuống LEAN (8 sản phẩm, 3 decision assets) và giữ automation ở L1 trước khi cắt bất kỳ mục 1–5 nào. Trust/legal pages không thuộc quota được phép cắt. Ghi scope change trong Decision Log.

