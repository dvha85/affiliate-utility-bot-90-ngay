# 02 — Curriculum và tài liệu nền tảng

## Cách học dành cho người mới hoàn toàn

Không học hết affiliate, SEO hay lập trình rồi mới bắt đầu. Mỗi module dùng cấu trúc:

```text
Khái niệm tối thiểu (20%)
→ Bài tập ngay trên dự án (60%)
→ Kiểm tra bằng đầu ra thật (20%)
```

Chỉ sang module sau khi làm được đầu ra kiểm chứng. Nếu một tài liệu dùng thuật ngữ lạ, tra [Glossary](11-glossary.md); không mở thêm mười khóa học cùng lúc.

### Cách cộng tác nếu bạn không biết code

Bạn không phải tự viết ứng dụng. Trong `Beginner Track`:

- **Bạn/chủ dự án** chọn ICP, kiểm nguồn, duyệt dữ liệu, terms, disclosure, chi phí và lần publish.
- **Coding agent** dựng giao diện tĩnh từ contract đã duyệt, chuyển CSV thành JSON, cài rule deterministic, chạy test và trả preview/evidence. Agent không tự đổi rule, nguồn, link hay tiêu chí đạt.
- **Research/content agent** chỉ tạo danh sách ứng viên hoặc draft gắn nguồn; output luôn vào staging/chờ duyệt.
- **Runtime** chỉ tính kết quả và mở direct link đã allowlist. Quyền chạy runtime khác với quyền tự sửa production.

Mặc định dùng CSV/JSON có version, một website tĩnh, không login/database/backend và direct approved links. Router phía server là phần tùy chọn sau khi chương trình cho phép và có người đủ khả năng security-review. Work Packet cụ thể nằm ở Day 43–56 trong [lộ trình](03-lo-trinh-90-ngay.md).

### Cách đọc các con số trong curriculum

Số sản phẩm, bằng chứng, tester, tỷ lệ pass và thời gian trong bộ tài liệu là **starter thresholds nội bộ cho bài tập 90 ngày**, không phải benchmark ngành, dự báo conversion hay bằng chứng thống kê. Chúng tạo kỷ luật và phạm vi. Chỉ thay một ngưỡng trước khi xem kết quả, ghi lý do/owner/ngày trong Decision Log; không hạ ngưỡng sau khi thấy số xấu.

## Bản đồ học–làm

| Module | Học để trả lời câu hỏi | Thực hành trên dự án | Bằng chứng qua môn |
|---|---|---|---|
| M0. Thực tế và an toàn | Bot tạo giá trị/tiền bằng cách nào? | Khóa phạm vi, ngân sách, quyền hạn | Project Charter được ký |
| M1. Funnel affiliate | Tiền đi qua những trạng thái nào? | Vẽ funnel và ví dụ số | Công thức được kiểm tra bằng máy tính |
| M2. Người dùng và vấn đề | Ai đang cần quyết định gì? | Thu thập bằng chứng nhu cầu | ICP + Problem Statement + Evidence Log |
| M3. Chương trình affiliate | Có được tham gia và quảng bá theo cách nào? | Đọc terms, payout, cookie, restrictions | Program Review, không còn hard blocker |
| M4. Dữ liệu có nguồn | Làm sao để bot không bịa? | Tạo schema, source register, freshness | 12–15 sản phẩm đạt completeness (`LEAN`: 8) |
| M5. Scoring và recommendation | Vì sao sản phẩm được đề xuất? | Tính bằng tay trên 5 tình huống | Kết quả giải thích được, không thiên commission |
| M6. Utility UX | Người mới có dùng được không? | Wireflow input → result → click | 5 scenario tests pass |
| M7. Xây MVP | Làm sao đưa utility lên web? | Build, deploy, rollback | URL public + runbook |
| M8. Tracking và economics | Rơi ở bước nào trong funnel? | Gắn event, test, đối soát network | Event QA + dashboard tuần |
| M9. Nội dung và organic discovery | Trang nào thật sự giúp người dùng? | Tạo 5 decision assets (`LEAN`: 3) | QA people-first + source/author/date |
| M10. Tự động hóa an toàn | Việc nào bot được tự làm? | Refresh/check/report có log | Hai chu kỳ chạy đúng + rollback test |
| M11. Thử nghiệm và quyết định | Khi nào sửa/giữ/scale/dừng? | Một thử nghiệm một biến | Decision memo dựa trên dữ liệu |

---

## M0 — Thực tế, đạo đức và hàng rào an toàn

### Bạn cần hiểu

- Bot không tạo ra nhu cầu; utility phải giải quyết một quyết định có thật.
- Affiliate là quan hệ có lợi ích tài chính và phải được công bố phù hợp.
- “Tự vận hành” là tự động hóa tác vụ nhỏ với giới hạn, không chuyển trách nhiệm pháp lý cho bot.
- Thu nhập chỉ được xác nhận ở `Final Commission`/`Payment`, không phải click hay order tạm.

### Bài tập

1. Viết một câu: “Tôi giúp [ai] chọn [loại công cụ] trong [ràng buộc] bằng [cơ chế khác biệt].”
2. Điền ngân sách trần và số giờ/tuần.
3. Liệt kê 10 việc bot không được tự làm.
4. Liệt kê điều kiện dừng khẩn cấp.

### Qua môn khi

- [ ] Các ô bắt buộc cho Day 7 trong [Project Charter](templates/01-project-charter.md) đã điền; trường chỉ có thể xác minh ở Day 14/28 được ghi `UNKNOWN`, owner và ngày quyết định, không đoán để lấp chỗ trống.
- [ ] Không có câu “bot sẽ tự kiếm tiền” nhưng thiếu người dùng, giá trị và funnel.
- [ ] Có ít nhất một ngày/tuần dành cho kiểm tra hệ thống.

---

## M1 — Mô hình affiliate và funnel tiền

### Từ vựng tối thiểu

`affiliate program`, `affiliate link`, `cookie window`, `attribution`, `click`, `order`, `valid order`, `reversal`, `final commission`, `payout`, `EPC`, `CVR`.

### Mô hình số

Không nhân các tỷ lệ có mẫu số khác nhau một cách tùy tiện. Xác định từng bước:

```text
Qualified Sessions
× Utility Start Rate
= Utility Starts

Utility Starts
× Utility Completion Rate
= Utility Completes

Recommendation Views
× Affiliate CTR
= Affiliate Clicks

Affiliate Clicks
× Order CVR
= Orders

Orders
× Validation Rate
× Finalization Rate
× Average Final Commission
= Expected Final Commission
```

### Ví dụ học tập, không phải benchmark

Giả sử 1.000 qualified sessions, 400 lượt xem recommendation, affiliate CTR 12%, order CVR 4%, validation 75%, finalization 90%, commission trung bình 20 USD:

```text
Clicks = 400 × 12% = 48
Orders = 48 × 4% = 1,92 (giá trị kỳ vọng, không phải có 1,92 đơn thật)
Expected Final Commission = 1,92 × 75% × 90% × 20 = 25,92 USD
```

Mục đích ví dụ là hiểu phép tính; tuyệt đối không dùng các tỷ lệ này làm dự báo của dự án.

### Bài tập và qua môn

- [ ] Vẽ funnel riêng của dự án.
- [ ] Ghi nguồn dữ liệu cho từng bước: website analytics hay affiliate dashboard.
- [ ] Tính ba kịch bản `thận trọng / cơ sở / tốt`, ghi rõ giả định.
- [ ] Có thể giải thích vì sao `Order` chưa phải doanh thu chắc chắn.

---

## M2 — ICP, vấn đề và search intent

### Khái niệm

- `ICP` ở đây là nhóm người có hoàn cảnh và quyết định tương tự, không chỉ tuổi/giới tính.
- `Problem Statement` mô tả tình huống, khó khăn và kết quả mong muốn.
- `Search intent` là việc người dùng thực sự muốn hoàn thành: học, so sánh, tính chi phí, chọn gói hoặc mua.
- Bằng chứng nhu cầu có thể đến từ query gợi ý, diễn đàn, review, changelog, bảng giá, tài liệu help và đối thủ. Bằng chứng cộng đồng chỉ là tín hiệu; không tự động sao chép nội dung.

### ICP khởi điểm cần thu hẹp

Không dùng “mọi người dùng AI”. Chọn một lát cắt, ví dụ:

```text
Solo affiliate site owner
Ngôn ngữ: [chọn]
Team: 1 người
Ngân sách phần mềm: [khoảng]
Mục tiêu: SEO + content + analytics
Nỗi đau: không biết phối hợp công cụ nào mà không vượt ngân sách
```

### Bài tập

1. Thu 30 bằng chứng nhu cầu từ tối thiểu 5 loại nguồn và 10 miền/trang khác nhau.
2. Chuẩn hóa mỗi bằng chứng: người dùng, việc cần làm, câu chữ gốc ngắn, URL, ngày, intent, độ tin cậy.
3. Gom thành 5–7 problem cluster.
4. Chấm cơ hội theo mẫu, chọn một cluster.

### Qua môn khi

- [ ] Không dùng search volume không rõ nguồn như sự thật.
- [ ] Có ít nhất 10 bằng chứng cho cluster được chọn.
- [ ] Có thể viết một đầu vào và đầu ra utility cụ thể.
- [ ] Có ba lý do vì sao utility tốt hơn một bài “Top 10”.

---

## M3 — Chương trình affiliate và compliance cơ bản

### Bạn cần đọc trong từng chương trình

- Ai đủ điều kiện tham gia, quốc gia được hỗ trợ.
- Kênh quảng bá được phép: website, email, social, paid search, coupon…
- Cấm bidding brand/trademark, self-referral, cookie stuffing, cloaking, misleading claims hay dùng coupon trái phép hay không.
- Cách tạo link và sub-ID; cookie/attribution window.
- Trạng thái order, validation/reversal/locking period.
- Tỷ lệ/cách tính commission; recurring có điều kiện gì.
- Ngưỡng và lịch payout; phương thức nhận tiền; KYC/tax form.
- Quy tắc dùng logo, screenshot, giá, trademark và API/feed.
- Quyền thay đổi điều khoản/chấm dứt; dữ liệu nào được tải xuống/lưu.

### Bài tập

- Điền [Affiliate Program Review](templates/04-affiliate-program-review.md) cho ít nhất 3 chương trình ứng viên.
- Chụp/lưu URL và ngày của terms; không lưu mật khẩu hoặc thông tin KYC trong repo.
- Viết một disclosure rõ nghĩa bằng ngôn ngữ của trang.
- Viết test: click nào được ghi nhận, cách tra sub-ID, khi nào gọi là final.

### Qua môn khi

- [ ] Có ít nhất một Program Review có `Decision=GO` và `Program Status=APPROVED`, hoặc kế hoạch launch bằng official non-affiliate links trong lúc `HOLD/APPLIED`.
- [ ] Không có nguồn thu duy nhất phụ thuộc một điều khoản chưa đọc.
- [ ] Biết chính xác việc nào bị cấm.
- [ ] Disclosure nằm gần recommendation/CTA trong prototype.

### Nguồn chính thức nên đọc

- [FTC — Endorsement Guides: What People Are Asking](https://www.ftc.gov/business-guidance/resources/ftcs-endorsement-guides-what-people-are-asking): phần affiliate giải thích disclosure phải rõ, dễ thấy và ở gần lời giới thiệu/link.
- [FTC — Disclosures 101](https://www.ftc.gov/business-guidance/resources/disclosures-101-social-media-influencers): cách công bố mối liên hệ vật chất bằng ngôn ngữ đơn giản.
- Terms và policy chính thức của **chính chương trình bạn chọn** luôn có ưu tiên vận hành cao hơn ví dụ chung trong bộ tài liệu.

---

## M4 — Data literacy và provenance

### Bạn cần hiểu

- `Schema`: danh sách trường và quy tắc của dữ liệu.
- `Provenance`: dữ liệu đến từ đâu, ai/điều gì kiểm tra, khi nào.
- `Freshness`: dữ liệu còn trong chu kỳ kiểm tra hay đã stale.
- `Confidence`: mức chắc chắn, tách khỏi điểm sản phẩm.
- `Null` khác `No`: chưa biết khác với không có tính năng.

### Bộ nguồn sự thật tối thiểu

1. [Products](templates/07a-products.csv): danh tính và trạng thái sản phẩm.
2. [Plans](templates/07b-plans.csv): giá/gói/vùng/currency/billing interval và pricing unit.
3. [Feature Evidence](templates/07c-feature-evidence.csv): claim/limitation gắn evidence.
4. [Source Registry](templates/06-source-registry.csv) và Program Review: quyền nguồn, điều khoản và status.
5. [Scoring Rules](templates/24-scoring-rules.csv): hard filter, component rubric 0–5 và deterministic reason.
6. [Stack Rules](templates/29-stack-rules.md): valid-stack floor, coverage, duplicate, compatibility và output selection.
7. [Dataset Release Manifest](templates/25-dataset-release-manifest.csv): exact files/versions, coverage, checksum, test và rollback cho một release.
8. [Recommendation Component Scores](templates/26-recommendation-component-scores.csv): rule/evidence nào tạo từng điểm của một lần chạy.

Mọi critical field phải truy được tới evidence ID/URL, `checked_at`, `checked_by`, confidence và `next_check_at`. Một URL cấp product không tự chứng minh mọi field.

### Freshness canonical cho một dataset release

- **Critical:** giá thực trả, billing/upfront, region, must-have feature, compatibility, hard constraint, link destination/status, program/disclosure rule và field có thể đổi eligibility/ranking. Coverage đang trong SLA phải là **100%**; một critical field `UNKNOWN/STALE/CONFLICT/BLOCKED` làm record/stack không đủ điều kiện public.
- **Noncritical:** mô tả hỗ trợ không làm đổi eligibility, total cost, ranking, disclosure hoặc destination. Coverage trong SLA phải đạt **≥98%**; phần còn lại phải ẩn hoặc gắn trạng thái rõ, không trình bày như dữ liệu mới.

Đây là release gate an toàn của dự án, không phải benchmark độ chính xác của toàn ngành.

### Bài tập

- Tạo 12–15 sản phẩm (`LEAN`: 8), gồm ít nhất 2 lựa chọn không affiliate.
- Với mỗi sản phẩm: một gói phù hợp, 4–6 feature quan trọng, audience fit, limitations và source.
- Nếu hỗ trợ team lớn hơn một người, ghi pricing model, price unit, included/minimum seats và cách tính actual charge; nếu không đủ evidence thì khóa MVP ở `team_size = 1`.
- Cố ý tạo một mâu thuẫn dữ liệu rồi thực hành đưa bản ghi về `BLOCKED`.
- Tạo manifest cho release thử; tính critical/noncritical completeness và freshness riêng.

### Qua môn khi

- [ ] Critical freshness/evidence coverage = 100%; noncritical freshness coverage ≥98% hoặc phần chưa đạt bị ẩn/gắn trạng thái.
- [ ] Không dùng `0` cho dữ liệu chưa biết.
- [ ] Bản ghi stale không được recommendation như dữ liệu mới.
- [ ] Dataset Manifest có exact schema/data/scoring/stack-rules versions, checksum, người duyệt và rollback version.

---

## M5 — Scoring và recommendation có thể giải thích

### Tách ba điểm

1. **User Fit Score:** lợi ích cho trường hợp người dùng.
2. **Evidence Confidence:** độ tin cậy của dữ liệu.
3. **Commercial Score:** kinh tế affiliate, chỉ để phân tích nội bộ và quyết định vận hành.

Commercial Score/commission không được dùng để chọn, sắp xếp, làm nổi bật hoặc phá hòa recommendation công khai. Khi User Fit bằng nhau, dùng thứ tự đã công bố: Evidence Confidence cao hơn → tổng monthly equivalent thấp hơn → actual/upfront charge thấp hơn → ít công cụ hơn → ID ổn định. Có thể hiển thị đồng hạng nếu khác biệt không có ý nghĩa.

### User Fit Score v0

Mỗi thành phần chấm 0–5 rồi quy đổi trọng số:

| Thành phần | Trọng số |
|---|---:|
| Phù hợp nhu cầu bắt buộc | 30% |
| Phù hợp ngân sách | 20% |
| Phù hợp quy mô/quy trình | 15% |
| Giá trị so với chi phí | 15% |
| Dễ triển khai/sử dụng | 10% |
| Khả năng tích hợp | 5% |
| Rủi ro/giới hạn đối với use case | 5% |

Hard filter chạy trước score: vượt ngân sách cứng, thiếu feature bắt buộc, không phục vụ thị trường hoặc evidence bị block thì loại khỏi kết quả.

### Anchor 0–5 và khả năng audit

Mỗi component phải có rubric riêng trong [Scoring Rules](templates/24-scoring-rules.csv). Khung chung chỉ dùng để viết rubric, không tự thay evidence:

| Điểm | Ý nghĩa chung |
|---:|---|
| 0 | Evidence xác nhận không đáp ứng hoặc có xung đột nghiêm trọng; hard constraint vẫn phải loại trước khi chấm. |
| 1 | Khoảng cách lớn, chỉ đáp ứng phần rất nhỏ của preference. |
| 2 | Đáp ứng một phần nhưng có trade-off đáng kể. |
| 3 | Đáp ứng mức cơ sở đã mô tả cho use case. |
| 4 | Đáp ứng mạnh, ít trade-off quan trọng. |
| 5 | Đáp ứng đầy đủ mức cao nhất đã định nghĩa và có evidence trực tiếp. |

`UNKNOWN` không phải điểm 0. Rule phải nói rõ `BLOCK`, `NO_SCORE` hay fallback nào được phép. Mỗi component score được lưu vào [Recommendation Component Scores](templates/26-recommendation-component-scores.csv) với rule ID và evidence IDs.

### Evidence Confidence tách khỏi Fit Score

Chấm confidence cho từng claim/field theo bốn thành phần đã định nghĩa trước, rồi lưu thành phần và evidence dùng để tính:

| Thành phần confidence | Trọng số khởi điểm | Anchor cần viết rõ |
|---|---:|---|
| Completeness | 40% | Các critical field bắt buộc có đủ evidence hay không |
| Freshness | 30% | Còn trong SLA đã duyệt hay đã stale |
| Source authority | 20% | Nguồn chính thức trực tiếp > tài liệu hỗ trợ chính thức > nguồn thứ cấp; community chỉ là tín hiệu |
| Consistency | 10% | Các nguồn độc lập có đồng thuận hay đang conflict |

Đây là trọng số khởi điểm nội bộ, phải được version như scoring rule. Điểm tổng không được che blocker: critical field `UNKNOWN/STALE/CONFLICT/BLOCKED` vẫn loại record/stack khỏi public dù confidence trung bình có vẻ cao. Confidence cho stack dùng mức thấp nhất của critical claims quyết định eligibility/cost/ranking, không dùng trung bình để che một claim yếu.

**Fit Score là điểm tương đối** trong candidate set + input + dataset/scoring/stack-rules versions hiện tại. Nó không phải xác suất mua thành công, chất lượng tuyệt đối, benchmark thị trường hay lời bảo đảm.

Ba output canonical:

- `Best Fit`: valid stack có Stack Fit cao nhất.
- `Cheapest Qualified`: stack rẻ nhất nhưng vẫn qua cùng Valid Stack Floor và minimum fit đã viết trước; không hạ chuẩn chỉ để có kết quả rẻ.
- `Alternative`: valid stack khác Best Fit và có trade-off có ý nghĩa, không phải vị trí dành cho commission cao hơn.

Chỉ hiển thị `Maximum Automation` khi có rubric automation 0–5, evidence và Golden Tests được duyệt cho toàn bộ candidate liên quan; nếu thiếu thì bỏ output này. Đây chỉ là nhãn một stack được đề xuất, không cấp L3 hay quyền tự thay production cho bot.

### Bài tập và qua môn

- [ ] Viết hard filters, component anchors và reason codes trong Scoring Rules trước khi code.
- [ ] Tính tay 5 scenario và lưu expected result.
- [ ] Có một scenario mà sản phẩm hoa hồng cao không đứng đầu.
- [ ] Kết quả nêu được 3 lý do chọn và ít nhất 1 giới hạn.
- [ ] Thay đổi một input quan trọng làm kết quả thay đổi hợp lý.
- [ ] Mỗi điểm/lý do truy được tới rule ID + evidence IDs; LLM chỉ diễn đạt reason đã tạo deterministic và output LLM được lưu để audit.

---

## M6 — UX của utility

### Luồng tối thiểu

```text
Landing
→ Affiliate disclosure ngắn
→ 5–8 câu hỏi, có giải thích
→ Kiểm tra input
→ Kết quả 1–3 stack
→ Lý do + assumptions + data date
→ Compare/alternative
→ CTA có disclosure
→ Feedback “hữu ích/không hữu ích”
```

### Nguyên tắc

- Không hỏi dữ liệu cá nhân nếu không cần cho recommendation.
- Không bắt đăng ký email để xem kết quả trong MVP.
- Không dùng countdown, nút giả, mặc định lựa chọn trả phí hoặc dark pattern.
- Nói “dựa trên dữ liệu đã xác minh ngày…” thay vì “tốt nhất tuyệt đối”.
- Mobile và keyboard phải dùng được; lỗi phải nói cách sửa.

### Qua môn khi

- [ ] Một người không đọc tài liệu vẫn hiểu cần nhập gì.
- [ ] Không có đường cụt.
- [ ] Recommendation vẫn hoạt động nếu không có affiliate link.
- [ ] CTA không che giấu đích đến hoặc quan hệ lợi ích.

---

## M7 — Khái niệm kỹ thuật tối thiểu để xây MVP

Bạn không cần trở thành lập trình viên. Beginner Track mặc định chỉ cần hiểu contract để giao cho coding agent và kiểm đầu ra:

```text
Approved/versioned CSV
→ deterministic build step tạo public JSON + manifest
→ static form/UI đọc đúng version
→ hard filters + scoring rules + stack rules
→ result/reasons từ rule IDs
→ direct approved product/affiliate link
→ custom click event
```

Không cần backend, database, login, scheduler hoặc router để launch MVP. Router `/go/{link_id}` chỉ thêm khi program cho phép, có security review và thật sự cần server-side click mapping.

Các khái niệm tối thiểu cần nhận biết:

- `Frontend`: phần người dùng nhìn và bấm.
- `Backend/API`: phần xử lý rule và dữ liệu phía sau.
- `Database`: nơi lưu bản ghi có cấu trúc.
- `Hosting/deploy`: đưa phiên bản lên Internet.
- `Domain/DNS`: địa chỉ và định tuyến tên miền.
- `Environment variable/secret`: nơi giữ khóa, không đưa vào mã nguồn.
- `Version control`: lịch sử thay đổi và khả năng rollback.
- `Staging/production`: nơi thử và nơi người dùng thật truy cập.
- `Log/monitor`: dấu vết để biết job chạy gì và lỗi ở đâu.

### Phân vai

- **Bạn:** cấp approved data/rules/link, duyệt preview, tạo tài khoản/domain/analytics, chấp nhận terms và bấm publish.
- **Coding agent:** build đúng Work Packet, không tự đổi business rule; trả URL preview, test report, versions, manifest và rollback instructions.
- **Runtime:** tự phục vụ cùng input + exact versions thành cùng output. Đây không phải quyền tự thay data/rule/link production.

### Bài tập và qua môn

- [ ] Vẽ được sơ đồ Browser → Static UI → Versioned Data/Rules → Direct Approved Link; router được đánh dấu `OPTIONAL`.
- [ ] Biết nơi backup dữ liệu và cách restore một bản test.
- [ ] Có staging hoặc preview trước production.
- [ ] Không có API key/affiliate credential trong file public.
- [ ] Có thể đưa Work Packet Day 43–56 cho coding agent và đối chiếu từng acceptance item mà không cần đọc code.

---

## M8 — Analytics, privacy và reconciliation

### Nguồn dữ liệu khác nhau

| Nguồn | Trả lời |
|---|---|
| Search Console | Trang có xuất hiện/có click trên Google không? |
| Web analytics | Người dùng làm gì trên site? |
| Link/event log | Click outbound nào site ghi; nếu bật router thì click nào đi qua redirect? |
| Affiliate dashboard | Network ghi nhận click/order/commission thế nào? |
| Sổ chi phí | Hệ thống tốn bao nhiêu? |

Không ép các nguồn phải bằng nhau; hiểu scope, múi giờ, ad blocker, consent, attribution và độ trễ.

### Events tối thiểu

`utility_start`, `utility_complete`, `result_view`, `comparison_open`, `product_outbound_click`, `feedback_submit`, `data_error_shown`.

`product_outbound_click` luôn tách `link_type = AFFILIATE | OFFICIAL_NON_AFFILIATE | FALLBACK` khỏi `delivery_mode = DIRECT | ROUTER`. Chỉ dẫn xuất `affiliate_click` khi `link_type=AFFILIATE` và offer version active tại thời điểm click; không gọi mọi outbound click là affiliate click.

### Nguồn chính thức nên đọc

- [Google Analytics — setup events](https://developers.google.com/analytics/devguides/collection/ga4/events).
- [GA4 — đo outbound clicks](https://support.google.com/analytics/answer/13566436).
- [Search Console — định nghĩa impression, position và click](https://support.google.com/webmasters/answer/7042828).

### Qua môn khi

- [ ] Test event xuất hiện trong chế độ debug/realtime phù hợp.
- [ ] Internal/test traffic có cách loại hoặc gắn cờ.
- [ ] Không gửi email, tên, input tự do hay dữ liệu nhạy cảm vào event.
- [ ] Có quy trình nhập/đối soát final commission từ network.

---

## M9 — SEO và nội dung people-first

### Học đúng trọng tâm

- Crawl, index và rank là ba việc khác nhau.
- Search Console impression/click khác website session/click affiliate.
- Nội dung hỗ trợ utility; không tạo trang chỉ để bắt biến thể từ khóa.
- AI có thể giúp nghiên cứu/cấu trúc/nháp, nhưng accuracy, originality, value và trách nhiệm vẫn thuộc bạn.
- Affiliate links nên được đánh dấu `rel="sponsored"` theo hướng dẫn của Google.

### Tài sản nội dung MVP

- `Standard`: 5 decision assets gồm utility, trang phương pháp/dữ liệu và ba asset use-case/comparison/calculator có intent khác nhau.
- `LEAN`: 3 decision assets gồm utility, trang phương pháp/dữ liệu và một asset quyết định ưu tiên cao nhất.
- Cả hai track đều cần trust pages phù hợp: About/Contact, Privacy và Affiliate Disclosure; trust pages không được tính bù vào decision assets.

Không publish đủ số bằng cách hạ chất lượng. Claim quan trọng phải truy được tới evidence; utility và trust pages có ưu tiên cao nhất.

### Nguồn chính thức nên đọc

- [Google — hướng dẫn dùng generative AI trên website](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content).
- [Google — spam policies, gồm scaled content abuse](https://developers.google.com/search/docs/essentials/spam-policies).
- [Google — qualify outbound links](https://developers.google.com/search/docs/crawling-indexing/qualify-outbound-links).
- [Google — bắt đầu với Search Console](https://support.google.com/webmasters/answer/10267942).

### Qua môn khi

- [ ] Mỗi trang có intent và job riêng.
- [ ] Claim quan trọng có nguồn và ngày.
- [ ] Không giả vờ đã dùng sản phẩm nếu chưa dùng.
- [ ] Không có trang gần như trùng nhau.
- [ ] Sitemap/canonical/noindex/robots được kiểm tra theo mục đích.

---

## M10 — Tự động hóa, QA và incident

### Mô hình an toàn

```text
Trigger
→ Fetch trong phạm vi được phép
→ Validate
→ Diff
→ Risk classification
→ Human approval nếu cần
→ Apply
→ Verify
→ Log
→ Alert/rollback khi lỗi
```

Tự động hóa đầu tiên nên là:

1. Link health check.
2. Đánh dấu data stale.
3. Tạo danh sách thay đổi cần duyệt.
4. Tổng hợp báo cáo KPI tuần.

Không bắt đầu bằng auto-publish hoặc auto-spend.

### Qua môn khi

- [ ] Job chạy lặp lại hai lần không tạo bản ghi trùng (`idempotent`).
- [ ] Có timeout, retry giới hạn và kill switch.
- [ ] Có owner khi cảnh báo.
- [ ] Thử rollback/restore thành công.

---

## M11 — Experiment và portfolio decision

### Một thử nghiệm hợp lệ

- Một câu hỏi.
- Một biến chính.
- Một metric chính và guardrail.
- Baseline, thời gian tối thiểu và điều kiện dừng viết trước.
- Segment rõ ràng.
- Không sửa giữa chừng chỉ vì số đẹp/xấu.

### Ví dụ

```text
Question: Rút form từ 7 xuống 5 câu có tăng utility completion không?
Primary metric: utility_complete / utility_start
Guardrail: tỷ lệ kết quả thiếu feature bắt buộc không tăng
Segment: người dùng mới, mobile
Decision date: ...
```

### Qua môn khi

- [ ] Không gọi 3 click là “chiến thắng”.
- [ ] `NOT_ENOUGH_DATA` được dùng khi chưa đủ quan sát.
- [ ] Quyết định nêu dữ liệu, giới hạn và ngày xem lại.
- [ ] Scale chỉ sau compliance/data/operations gate, không chỉ conversion gate.

## Bài kiểm tra cuối curriculum

Bạn sẵn sàng launch khi có thể trả lời không cần đoán:

1. Utility giúp ai quyết định gì?
2. Vì sao kết quả A đứng trên B?
3. Mỗi giá/feature/term đến từ đâu và kiểm tra khi nào?
4. Người dùng biết bạn có thể nhận commission ở đâu?
5. Click nào được ghi ở site, click/order nào được network ghi?
6. Một order bị hủy sẽ thay đổi dashboard thế nào?
7. Nếu job cập nhật sai hàng loạt, bấm gì để dừng và quay lại?
8. Khi nào trạng thái là FAIL, khi nào chỉ là NOT_ENOUGH_DATA?
9. Bot đang được phép làm gì, và tuyệt đối không được làm gì?
10. Nếu không có commission sau 90 ngày, bằng chứng nào quyết định sửa hay tiếp tục?

Nếu chưa trả lời được câu nào, quay lại module tương ứng; không bù bằng cách xuất bản nhiều trang hơn.

