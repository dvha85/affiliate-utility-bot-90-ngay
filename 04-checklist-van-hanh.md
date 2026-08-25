# 04 — Checklist vận hành

> Cách dùng: sao chép checklist phù hợp vào worklog, điền ngày và bằng chứng. Dấu tick không có URL/file/log đi kèm chỉ là lời tự xác nhận, chưa phải bằng chứng.

## A. Checklist mỗi buổi làm việc

```text
Ngày:
Day số:
Mục tiêu duy nhất hôm nay:
Timebox:

TRƯỚC KHI LÀM
[ ] Đã đọc row tương ứng trong lộ trình Day 1–90
[ ] Đã kiểm tra việc phụ thuộc từ hôm trước
[ ] Không có cảnh báo P0/P1 chưa xử lý
[ ] Biết đầu ra phải lưu ở đâu
[ ] Việc hôm nay không cần quyền/chi phí chưa được duyệt

TRONG KHI LÀM
[ ] Fact quan trọng được ghi kèm nguồn và checked_at
[ ] Giả định được gắn nhãn ASSUMPTION
[ ] Không đưa password, API key, KYC, thông tin ngân hàng vào tài liệu/prompt
[ ] Không mở rộng sang ngách/tính năng ngoài phạm vi
[ ] Ghi blocker ngay khi gặp, không âm thầm bỏ qua

TRƯỚC KHI KẾT THÚC
[ ] Đầu ra tồn tại và mở được
[ ] Đã tự kiểm theo điều kiện đạt của ngày
[ ] Đã lưu evidence/link/log
[ ] Đã cập nhật trạng thái PASS / FAIL / BLOCKED / NOT_ENOUGH_DATA
[ ] Đã ghi bước đầu tiên cho ngày mai
[ ] Đã ghi thời gian và chi phí phát sinh
```

## B. Checklist review cuối tuần

```text
Tuần:
Khoảng ngày:
Reviewer:

DELIVERY
[ ] Mỗi ngày đã làm có đầu ra, không chỉ ghi “đã nghiên cứu”
[ ] Milestone tuần đã đạt hoặc có rework plan
[ ] Scope không tăng ngoài Decision Log
[ ] Backlog có owner và priority

DATA & TRUST
[ ] Fact/price/feature/term mới có source + checked_at
[ ] UNKNOWN không bị biến thành 0/false
[ ] Conflict và stale records không được public như fact mới
[ ] Claim trải nghiệm phản ánh đúng việc đã làm thật

QUALITY
[ ] Đã lấy mẫu ít nhất 5 records/pages/runs để kiểm thủ công
[ ] Golden Tests vẫn pass sau thay đổi logic/data
[ ] Không còn P0/P1
[ ] Backup gần nhất tồn tại; restore date còn trong lịch

FUNNEL & MONEY (SAU LAUNCH)
[ ] Search Console, analytics, router và network được xem riêng
[ ] Test/internal traffic được gắn cờ hoặc loại
[ ] Ordered / Valid / Final / Paid không bị gộp
[ ] Cohort chưa mature được ghi NOT_ENOUGH_DATA

OPERATIONS
[ ] Job fail/alert đã có owner
[ ] Không job nào vượt budget/rate limit/quyền cho phép
[ ] Không có secret hoặc direct identifier/Google-prohibited PII trong log/URL/sub-ID; mọi identifier giả danh còn lại đã nằm trong Data Inventory, có mục đích và retention
[ ] Chỉ chọn tối đa 1–2 thử nghiệm cho tuần mới

DECISION
[ ] Điều đã biết:
[ ] Điều chưa biết:
[ ] Quyết định:
[ ] Evidence:
[ ] Việc tuần tới:
[ ] Người chịu trách nhiệm + ngày xem lại:
```

## C. Checklist duyệt một nguồn dữ liệu mới

### Quyền truy cập và sử dụng

- [ ] Ghi URL, chủ sở hữu, loại nguồn, mục đích sử dụng thương mại.
- [ ] Ưu tiên API/feed/export/asset pack chính thức.
- [ ] Trước mỗi crawl, tải hoặc dùng cache `robots.txt` còn hiệu lực tối đa 24 giờ và đúng user-agent; `Disallow` áp dụng là `NO-GO` nội bộ cho crawler đó.
- [ ] Nếu lấy `robots.txt` lỗi mạng/5xx hoặc không xác định được rule, job fail closed như bị disallow và chuyển human review; không suy diễn là được phép.
- [ ] Đọc Terms of Service, license và API docs; lưu URL/version/date.
- [ ] Xác nhận automated access, cache, commercial reuse, attribution và rate limit.
- [ ] Xác nhận quyền dùng logo, screenshot, text, rating/review và data compilation.
- [ ] Không cần bypass login, paywall, CAPTCHA hoặc technical control.
- [ ] `401/403/429` làm job dừng và chuyển human review; không tăng tốc/đổi IP để né.
- [ ] Hiểu rằng robots `Allow`/không có robots không phải giấy phép sao chép hoặc tái sử dụng.

### Thiết kế vận hành

- [ ] Chỉ lấy fields thực sự cần.
- [ ] Có user-agent rõ, contact và rate limit thấp khi crawl được phép.
- [ ] Có cache, timeout, retry giới hạn với backoff.
- [ ] Có raw snapshot/evidence reference nhưng không lưu nội dung vượt quyền.
- [ ] Có `last_checked_at`, `next_check_at`, owner và SLA.
- [ ] Có kill switch riêng cho nguồn.
- [ ] Nguồn mới được con người duyệt trước khi bot truy cập định kỳ.

### NO-GO ngay nếu

- [ ] ToS/robots/license cấm phương thức dự kiến.
- [ ] Quyền thương mại/tái sử dụng mơ hồ đối với dữ liệu cốt lõi.
- [ ] Phải đăng nhập trái phép, vượt CAPTCHA/paywall hoặc né rate limit.
- [ ] Dữ liệu chứa PII/sensitive data không cần thiết.
- [ ] Không thể chứng minh provenance cho field sẽ public.

## D. Checklist thẩm định một affiliate program

### Eligibility và tiền

- [ ] Trang chương trình và agreement là nguồn chính thức.
- [ ] Quốc gia của bạn được tham gia; target market được phục vụ.
- [ ] Phương thức KYC, tax form, payout và currency khả dụng.
- [ ] Commission type/rate/base tính rõ.
- [ ] Cookie/attribution window và first/last-click rule rõ.
- [ ] Validation/locking/reversal/clawback rõ.
- [ ] Minimum payout, payment schedule và termination balance rõ.

### Phương thức quảng bá

- [ ] Domain/site/channel đã khai đúng và được chấp thuận nếu yêu cầu.
- [ ] Organic SEO/content được phép.
- [ ] AI-assisted content/automation được phép hoặc không bị terms cấm.
- [ ] Quy định PPC/direct link/brand bidding được ghi.
- [ ] Quy định email/SMS/social/video/coupon/incentive được ghi.
- [ ] Self-referral, client/employee referral được ghi.
- [ ] Redirect/cloaking/sub-ID/deep link được ghi.
- [ ] Không có điều khoản trọng yếu còn “tôi đoán là được”.

### Assets, tracking và vận hành

- [ ] Quyền dùng trademark/logo/screenshot/copy/price/review/API/feed rõ.
- [ ] Link format lấy từ dashboard/API chính thức.
- [ ] Sub-ID/clickref được phép và biết giới hạn ký tự/PII.
- [ ] Có API/webhook/export và hiểu độ trễ nếu dùng.
- [ ] Biết cách test được phép; không tự click/mua để thử.
- [ ] Có lịch rà terms tối thiểu hàng tháng và khi nhận notice.
- [ ] Có backup offer/program nếu merchant dừng.

### Quyết định

- [ ] `GO`: `Decision=GO`, `Program Status=APPROVED`, mọi blocker đã rõ và con người duyệt.
- [ ] `HOLD`: còn câu hỏi; có thể soft-launch bằng link chính thức `OFFICIAL_NON_AFFILIATE` nếu nguồn/link được duyệt, nhưng chưa đặt link affiliate.
- [ ] `NO-GO`: không hỗ trợ quốc gia/payout/channel hoặc rủi ro không chấp nhận.

## E. Checklist một product record hoàn chỉnh

- [ ] `product_id`, tên, vendor, official URL, category.
- [ ] Status `ACTIVE/DISCONTINUED/UNKNOWN`.
- [ ] Region/language/platform availability.
- [ ] Mỗi plan có currency, price, billing interval, actual charge.
- [ ] Pricing theo team có `pricing_model`, `per_seat_amount`, `included_seats`, `minimum_seats` và usage tiers khi áp dụng; nếu chưa đủ dữ liệu thì khóa recommendation về solo/one-seat.
- [ ] Nếu quy đổi annual sang tháng, ghi rõ “equivalent”, không gọi monthly-billed.
- [ ] Tax/VAT assumption được nêu khi ảnh hưởng total.
- [ ] Trial/refund chỉ ghi khi có official source.
- [ ] 5–10 critical features dùng schema chuẩn.
- [ ] Must-have limitations và incompatibilities.
- [ ] Audience/use-case fit có lý do.
- [ ] Mỗi critical field có source ID + checked_at + confidence.
- [ ] Unknown/conflict/stale được biểu diễn rõ.
- [ ] Changelog lưu before/after và effective/detected/verified timestamps.
- [ ] `next_check_at` được đặt.

## F. Checklist trước khi thay scoring/recommendation

- [ ] Viết vấn đề người dùng hoặc data issue cần sửa.
- [ ] Tách hard filter, User Fit, Evidence Confidence và Commercial Score.
- [ ] Commission không vào User Fit Score.
- [ ] Commercial Score không chọn, sắp xếp, làm nổi bật hoặc phá hòa recommendation công khai.
- [ ] Rule/weight mới có version.
- [ ] Tính expected output trước khi chạy.
- [ ] Chạy toàn bộ Golden Test Set.
- [ ] 100% hard constraints pass.
- [ ] No-result, tie, boundary, missing và stale cases pass.
- [ ] Top-result changes được liệt kê và review thủ công.
- [ ] Một lựa chọn non-affiliate tốt hơn vẫn có thể đứng đầu.
- [ ] Có rollback version.
- [ ] Con người duyệt trước khi production đổi.

## G. Definition of Done trước publish mỗi trang

### Giá trị cho người dùng

- [ ] Một audience, intent và decision job rõ.
- [ ] Có giá trị gốc: utility, calculator, dataset, method, test hoặc comparison thật.
- [ ] Không copy/spin merchant copy hoặc tạo trang gần trùng để bắt từ khóa.
- [ ] Pros/cons và trường hợp không phù hợp được nêu.
- [ ] “Best” chỉ dùng khi có method/criteria/evidence rõ.

### Dữ kiện và tính trung thực

- [ ] Critical claims có source chính thức + verified date.
- [ ] Price đúng region/currency/billing period.
- [ ] Không viết như đã dùng/test nếu chưa thật sự dùng/test.
- [ ] Không có fake quote, fake rating, fake user hay review do AI bịa.
- [ ] Page hiển thị last data check và limitations.
- [ ] Human fact-check toàn trang.

### Affiliate

- [ ] Disclosure cùng ngôn ngữ trang, rõ trên mobile, trước/gần recommendation và CTA đầu.
- [ ] Disclosure nói rõ có thể nhận hoa hồng; không chỉ ghi “affiliate link”.
- [ ] Mỗi paid/affiliate link có thuộc tính qualifying được duyệt; chuẩn nội bộ mặc định là `rel="sponsored"` (Google cũng chấp nhận `nofollow` để đánh dấu paid link, nhưng ngoại lệ phải được ghi).
- [ ] Offer active, đúng merchant/product/affiliate ID/destination.
- [ ] Không tự thêm UTM/sub-ID/redirect bị program cấm.
- [ ] Không ám chỉ merchant tài trợ/xác nhận nếu không có.

### Privacy, UX và kỹ thuật

- [ ] Privacy/cookie choice khớp đúng tags đang chạy và thị trường đã chọn.
- [ ] Không direct identifier/Google-prohibited PII trong analytics, URL hoặc sub-ID; session/click ID giả danh đã được inventory, tối thiểu hóa và có retention.
- [ ] Đã thử `accept`, `deny`, `revoke` và lần quay lại; request thực tế khớp thiết kế consent/tag Basic hoặc Advanced đã được phê duyệt.
- [ ] Form có validation và no-result/error path.
- [ ] Keyboard/mobile/contrast cơ bản pass.
- [ ] Title/description/canonical/indexing intent đúng.
- [ ] Internal links và source links hoạt động.
- [ ] Events được QA bằng debug/realtime; không phát trùng.
- [ ] Page/data/scoring version và rollback target được lưu.
- [ ] Reviewer ký `PUBLISH`; nếu thiếu bất kỳ mục critical thì `HOLD`.

## H. Checklist launch Day 67–70

```text
SCOPE
[ ] Một ICP, một utility, một language/market
[ ] Không còn feature “hay thì có” chặn launch

FUNCTION
[ ] Golden Tests pass theo gate
[ ] No-result/error/mobile/keyboard pass
[ ] 0 P0/P1

DATA
[ ] 100% public critical fields có evidence
[ ] Không conflict public
[ ] Freshness đạt gate
[ ] Source permissions còn hợp lệ

TRUST & COMPLIANCE
[ ] About, Methodology/Data Sources, Editorial/Update Policy
[ ] Affiliate Disclosure, Privacy, Contact và policy cần thiết
[ ] Disclosure gần CTA; paid link có qualifying rel đã duyệt (mặc định `sponsored`)
[ ] Program/domain/channel/link đã được duyệt
[ ] Không claim trải nghiệm giả hoặc scaled thin content

TRACKING
[ ] Search Console property ownership
[ ] Sitemap fetch được; hiểu submit không bảo đảm index
[ ] journey_id tạo ở utility_start; recommendation_run_id tạo khi submit
[ ] utility_start / complete / result_view / product_outbound_click pass
[ ] affiliate_click chỉ có khi link_type=AFFILIATE
[ ] Test/internal traffic có cờ
[ ] Direct-link event log hoặc router log và Link Registry pass
[ ] Nếu affiliate active: network reconciliation plan tồn tại; nếu soft-launch: `NOT_APPLICABLE_YET` + test plan

OPERATIONS
[ ] Backup + restore test
[ ] Logs/alerts/owner/kill switch
[ ] Secrets tách và MFA bật
[ ] Budget monitor
[ ] Incident runbook
[ ] Người launch và rollback authority rõ

SIGN-OFF
[ ] Functional owner
[ ] Data owner
[ ] Compliance/privacy owner
[ ] Business owner
[ ] Launch timestamp + baseline snapshot
```

## I. Checklist bot hằng ngày sau launch

```text
[ ] Scheduled jobs kết thúc và có run_id
[ ] Không source critical quá SLA
[ ] Không schema/validation conflict chưa chặn
[ ] Không material change được auto-publish
[ ] Active offers/destinations healthy theo phương thức test được phép
[ ] Event volume/required fields/duplicates trong guardrail
[ ] Network import hoàn thành hoặc ghi delay
[ ] Retry không tạo duplicate
[ ] Không auth/rate-limit error chưa xử lý
[ ] Không secret/PII xuất hiện trong log
[ ] Run summary được tạo
[ ] Alert P0/P1 đã gửi đúng owner
```

## J. Checklist con người 10–15 phút sau launch

```text
[ ] Xem alert P0/P1 và sự cố đang mở
[ ] Duyệt/từ chối material data changes
[ ] Kiểm job fail sau retry
[ ] Kiểm spike/drop traffic hoặc click bất thường
[ ] Kiểm order/commission/reversal bất thường
[ ] Kiểm program/fraud/terms notice
[ ] Gán owner + deadline cho exception
[ ] Xác nhận không vượt budget
```

## K. Checklist hằng tháng

```text
MONEY
[ ] Chốt Ordered / Valid / Final / Paid riêng
[ ] Đối soát payout với bằng chứng nhận tiền
[ ] Ghi fee, FX và chênh lệch
[ ] Xem pending aging, reversal và payout overdue

PROGRAM & SOURCE
[ ] Diff affiliate agreements/policies/rates/cookie windows
[ ] Audit approved domains/channels/link formats
[ ] Audit source ToS/license/robots/API limits
[ ] Retire source/offer/program không còn phù hợp

DATA & CONTENT
[ ] Freshness/evidence/disclosure coverage
[ ] Rà top pages và pages stale
[ ] Không chỉ đổi “last updated” mà chưa re-verify
[ ] Lấy mẫu content bot output để kiểm fact/original value

SECURITY & PRIVACY
[ ] Review access, service accounts, API keys và processors
[ ] Backup + sample restore
[ ] Data inventory, retention/deletion và PII scan
[ ] Consent behavior khớp tag configuration

PORTFOLIO
[ ] Chi phí + giờ vận hành
[ ] Exception/intervention per 100 bot runs
[ ] Thử nghiệm đã mature
[ ] Quyết định maintain/iterate/scale/hold/stop có log
```

## L. Dừng khẩn cấp ngay khi

- [ ] Lộ secret/API key hoặc có truy cập trái phép.
- [ ] Redirect sai merchant/domain hoặc link bị chiếm.
- [ ] Giá/feature/hard constraint sai diện rộng.
- [ ] Event/order/commission bị nhân đôi.
- [ ] Bot truy cập nguồn trái terms/robots/quyền đã duyệt.
- [ ] Affiliate account/traffic nhận fraud hoặc compliance warning.
- [ ] Disclosure bị mất trên trang có affiliate link.
- [ ] Non-essential tracking chạy trái consent decision nơi cần consent.
- [ ] Recommendation ranking bị commission chi phối.
- [ ] Không thể đối soát một chênh lệch tiền nghiêm trọng.

Hành động: bật kill switch cho phần bị ảnh hưởng → giữ log/evidence → rollback last-known-good → gán severity/owner → chỉ mở lại sau QA và phê duyệt.

