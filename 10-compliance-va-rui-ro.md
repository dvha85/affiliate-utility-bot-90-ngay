# 10 — Compliance và quản trị rủi ro

> Ngày kiểm chứng nguồn: 25/08/2026. Đây là checklist vận hành và giáo dục, không phải tư vấn pháp lý/thuế. Luật, hướng dẫn và affiliate terms có thể đổi; phải kiểm tra lại theo nơi bạn vận hành, thị trường/người dùng thực tế và từng chương trình trước launch/market mới.

## 1. Bảy lớp phải kiểm tra riêng

```text
1. Luật nơi chủ dự án/đơn vị vận hành
2. Luật nơi người dùng mục tiêu/thực tế ở
3. Luật quảng cáo/consumer protection/disclosure
4. Luật privacy/cookies/data transfer/security
5. Affiliate agreement + program policies
6. Quyền nguồn dữ liệu, IP, trademark, review/assets
7. Search/analytics/hosting/vendor platform policies
```

Qua một lớp không có nghĩa qua các lớp khác. Ví dụ:

- `robots.txt` cho crawl không phải giấy phép copy/reuse.
- Được program nhận không có nghĩa mọi channel đều được phép.
- `rel="sponsored"` cho Google không thay disclosure cho người đọc.
- Cookie banner không tự chứng minh mọi tag/data flow hợp pháp.
- Trang công khai không đồng nghĩa dữ liệu/nội dung được tự do bán lại.

## 2. Affiliate disclosure — hai việc độc lập

### Cho người đọc

FTC hướng dẫn material connection phải được công bố rõ và dễ thấy; trong FAQ affiliate, disclosure nên ở gần recommendation/link, và chỉ ghi “affiliate link” hoặc một nút “buy now” có thể không đủ để người đọc hiểu bạn được trả hoa hồng. Xem [FTC Endorsement Guides FAQ](https://www.ftc.gov/business-guidance/resources/ftcs-endorsement-guides-what-people-are-asking), [Disclosures 101](https://www.ftc.gov/business-guidance/resources/disclosures-101-social-media-influencers) và [16 CFR Part 255](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-B/part-255).

Mẫu tiếng Việt:

> Tôi có thể nhận hoa hồng nếu bạn mua qua một số liên kết trên trang này.

Thông báo bổ sung ngay gần một link:

> Tôi có thể nhận hoa hồng nếu bạn mua qua liên kết này.

Thông báo cạnh link chỉ là bổ sung, không thay disclosure đầy đủ ở gần recommendation.

Mẫu tiếng Anh cho trang tiếng Anh:

> I may earn a commission if you purchase through some links on this page.

Checklist:

- [ ] Cùng ngôn ngữ nội dung.
- [ ] Trước/gần recommendation và affiliate link đầu; vẫn rõ trên mobile.
- [ ] Không chỉ footer, About, hover, hyperlink hoặc sau “Read more”.
- [ ] Nói rõ có thể nhận tiền/hoa hồng, không chỉ thuật ngữ “affiliate”.
- [ ] Lặp/điều chỉnh theo định dạng; video có endorsement bằng hình/tiếng cần disclosure phù hợp với cách người xem nhận nội dung.
- [ ] Không nói độc lập/không có lợi ích nếu đang nhận commission.

FTC lưu ý người đăng ngoài Mỹ vẫn cần cân nhắc luật Mỹ khi có thể dự đoán hợp lý nội dung ảnh hưởng người tiêu dùng Mỹ, đồng thời luật nước khác có thể áp dụng. Đây là hướng dẫn của Hoa Kỳ, không phải safe harbor toàn cầu và không nên sao chép nguyên mẫu disclosure sang mọi thị trường.

Với EU/EEA, [European Commission Influencer Legal Hub](https://commission.europa.eu/topics/consumers/consumer-rights-and-complaints/influencer-legal-hub_en) nêu affiliate marketing là advertising và phải được công bố. Với UK, [ASA/CAP Affiliate Marketing guidance](https://www.asa.org.uk/advice-online/affiliate-marketing.html) yêu cầu affiliate advertising phải dễ nhận biết; tùy toàn bộ hay chỉ một phần nội dung là affiliate, có thể cần nhãn `Ad` trước khi người dùng tương tác và nhận diện từng phần/link. ASA/CAP cũng cảnh báo câu chung “may earn a commission” có thể mơ hồ trong một số bối cảnh UK. Vì vậy phải review copy, vị trí và phạm vi theo thị trường, media và facts thực tế.

### Cho Google Search

Google hướng dẫn đánh dấu paid/affiliate links bằng `rel="sponsored"`; `nofollow` vẫn là cách được chấp nhận để flag các link này nhưng `sponsored` được ưu tiên. Dự án dùng `sponsored` làm **mặc định nội bộ**; một ngoại lệ legacy dùng `nofollow` phải có lý do và người duyệt trong Link Registry. Xem [Qualify outbound links](https://developers.google.com/search/docs/crawling-indexing/qualify-outbound-links) và [affiliate link reminder](https://developers.google.com/search/blog/2021/07/link-tagging-and-link-spam-update).

```html
<a href="/go/LNK-001" rel="sponsored">Xem sản phẩm</a>
```

Disclosure cho người và thuộc tính qualifying link cho Google là hai control độc lập. Mỗi affiliate link phải có disclosure phù hợp và `rel="sponsored"` theo mặc định dự án; `nofollow` chỉ là ngoại lệ đã duyệt, không phải lý do bỏ disclosure.

### Terminology và URL storage trong Link Registry

- Mỗi target có `link_id` ổn định. `offer_id` là business offer tham chiếu nếu có, không thay `link_id`.
- `link_type`: `AFFILIATE / OFFICIAL_NON_AFFILIATE / FALLBACK`.
- `delivery_mode`: `DIRECT / ROUTER`. Đây là chiều độc lập với `link_type`; không dùng giá trị gộp như `AFFILIATE_ROUTER`.
- Với affiliate link, `affiliate_url_storage_type` là `PUBLIC_VALUE` hoặc `SECRET_REFERENCE`; chỉ một trong `affiliate_url_public_value`/`affiliate_url_secret_reference` được có giá trị. Link không affiliate dùng `NONE` và để trống cả hai.
- `affiliate_url_fingerprint` phục vụ đối chiếu/version nhưng không thay secret storage, access control hoặc redaction. URL affiliate không mặc nhiên là secret; phân loại theo credential/token, agreement và risk thực tế.
- `public_href` là giá trị gắn vào CTA: direct dùng URL đã duyệt để public, router dùng `/go/{link_id}`. Router chỉ lookup link allowlisted, không nhận arbitrary destination từ user.
- Field nhiều giá trị dùng quoted JSON array trong cột hậu tố `_json` hoặc child table; không dùng pipe/comma list tự do.

## 3. Review, testimonials và claim trung thực

### Quy tắc cứng

- Không nói “tôi đã dùng/test” nếu chưa dùng/test theo cách hỗ trợ claim.
- Không tạo user quote, rating, số người dùng, testimonial hoặc case study bằng AI.
- Không thay đổi review làm sai ý nghĩa.
- Không trả thưởng với điều kiện review phải tích cực/tiêu cực.
- Không gọi một site/score là độc lập nếu thương hiệu kiểm soát mà không công bố.
- Không nói “best” nếu không có tiêu chí, method, evidence và đối tượng cụ thể.
- Claim định lượng/pháp lý/sức khỏe/tài chính cần loại bằng chứng và review phù hợp; MVP này nên tránh lĩnh vực rủi ro cao.

[FTC Consumer Reviews and Testimonials Rule, 16 CFR Part 465](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-D/part-465) có hiệu lực từ 2024 và cấm nhiều hành vi liên quan fake/false reviews, incentive theo sentiment, review suppression và fake influence. Đọc thêm [FTC Q&A chính thức](https://www.ftc.gov/business-guidance/resources/consumer-reviews-testimonials-rule-questions-answers).

### Nếu chưa dùng SaaS trực tiếp

Gọi đúng loại nội dung:

> “So sánh dựa trên tài liệu, bảng giá và thông tin công khai đã kiểm tra ngày…”

Không gọi đó là hands-on review. Google cũng khuyến nghị review chất lượng cao dùng góc nhìn người dùng, bằng chứng trải nghiệm khi claim trải nghiệm, đo lường, khác biệt và pros/cons; xem [Write high-quality reviews](https://developers.google.com/search/docs/specialty/ecommerce/write-high-quality-reviews).

## 4. Nội dung AI, thin affiliate và scaled content

Google không cấm nội dung chỉ vì dùng AI. Rủi ro là tạo nhiều trang chủ yếu để thao túng ranking, không giúp người dùng; spam policies nêu GenAI hàng loạt không giá trị, scrape/ghép/biến đổi nội dung không thêm giá trị và `thin affiliation`. Xem:

- [Spam policies](https://developers.google.com/search/docs/essentials/spam-policies).
- [Guidance dùng generative AI](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content).
- [People-first content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content).
- [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide).

### Publish gate

- [ ] Trang giải quyết một decision job có thật.
- [ ] Có original value: utility, calculator, normalized data, method, test hoặc analysis.
- [ ] Merchant descriptions không bị copy/spin.
- [ ] Không tạo một trang cho mọi query variation/fan-out chỉ để rank.
- [ ] Who/How/Why, author/reviewer, method và update date phù hợp.
- [ ] Fact/meta/structured data/alt text đều được fact-check.
- [ ] Nếu automation đáng để người đọc biết, mô tả cách dùng một cách trung thực; không có quy tắc chung bắt mọi AI text phải dán một nhãn giống nhau.

## 5. Quyền thu thập và sử dụng dữ liệu

### Bốn kiểm tra song song

```text
ROBOTS
+ TERMS/LICENSE/API RULES
+ COPYRIGHT/DATABASE/TRADEMARK/ASSET RIGHTS
+ PRIVACY/PERSONAL DATA
= quyết định nguồn có dùng được hay không
```

[RFC 9309](https://www.rfc-editor.org/rfc/rfc9309.html) chuẩn hóa Robots Exclusion Protocol và nêu robots rules không thay access authorization/security. Vì vậy:

- Chính sách nội bộ của dự án: nếu rule áp cho đúng user-agent và target URL là `Disallow` thì bot crawler dừng. Đây là protocol control, không phải kết luận pháp lý toàn diện.
- `Allow`/không có robots → vẫn phải kiểm ToS/license/rights/privacy.
- Ưu tiên API/feed/export/asset pack chính thức hoặc xin phép bằng văn bản.

### Pre-flight robots cho mỗi crawl

- Fetch `robots.txt` trước run hoặc dùng cache còn hiệu lực; theo RFC 9309, crawler không nên dùng cache quá 24 giờ trừ khi robots không thể truy cập.
- Khi fetch thành công, chọn đúng group/user-agent và áp rule khớp target URL; không kết luận chỉ vì file có một dòng `Disallow` ở scope khác.
- Nếu `robots.txt` trả `5xx` hoặc lỗi mạng làm server unreachable, fail closed: giả định complete disallow và dừng crawl. Không dùng bản cũ vô thời hạn.
- Chính sách dự án với `401/403/429` là dừng và human review, không đổi IP/user-agent hoặc tăng retry để né.
- Lưu `last_fetched_at`, HTTP status, rules hash/version, `cache_expires_at`, decision và evidence; thay đổi material tạo Change Request trước khi mở lại nguồn.

### Checklist crawler/source

- [ ] [Source Registry](templates/06-source-registry.csv) có owner, URL, purpose, fields, rights, version/validity, robots decision và SLA; claim/field quan trọng nối tới [Source Evidence](templates/07d-source-evidence.csv).
- [ ] Không bypass login/paywall/CAPTCHA/technical control.
- [ ] User-agent rõ, contact, low rate, cache, timeout, bounded retry/backoff.
- [ ] Robots decision còn hiệu lực không quá 24 giờ cho scheduled crawl; `5xx`/unreachable fail closed.
- [ ] `401/403/429` → stop + human review, không né.
- [ ] Không copy nguyên merchant text; fact có thể khác quyền với cách diễn đạt/database.
- [ ] Không dùng logo/screenshot/review/rating nếu chưa có quyền.
- [ ] Không thu personal/sensitive data ngoài mục đích tối thiểu đã review.
- [ ] Có kill switch; robots được kiểm theo pre-flight ở trên, còn manual terms/license/program review chạy tối thiểu hằng tháng và ngay khi có notice/change signal.

Nếu bất kỳ quyền trọng yếu nào mơ hồ, chuyển `MANUAL_ONLY/HOLD`, dùng official link/source thủ công hoặc xin phép; không “crawl trước hỏi sau”.

## 6. Affiliate program-specific rules

Không có một bộ rules chung thay terms từng program. Mỗi program cần [Program Review](templates/04-affiliate-program-review.md), một record có version/validity trong [Program Registry](templates/04b-program-registry.csv) và evidence cho từng điều khoản trọng yếu.

### Những mục phải đọc

- Approved properties/domains/channels.
- Network agreement, vendor/channel agreement, program policies, thứ tự ưu tiên khi mâu thuẫn và đúng version/effective date.
- Ai đã accept terms, lúc nào, bằng account/property nào và evidence reference; bot không tự accept.
- SEO/PPC/direct link/brand bidding.
- Email/SMS/social/video/coupon/incentive.
- Self/client/employee referral.
- Link format/sub-ID/cloaking/redirect/referrer.
- Artificial clicks, cookie stuffing, fake/automated leads.
- Trademark/logo/screenshots/copy/prices/reviews/API/cache.
- Cookie/pixel/consent/privacy notice, restricted data, processor/data-transfer và retention/deletion obligations.
- API/webhook/postback/server-to-server conversion upload: fields được gửi, authentication, permitted purpose và PII/personal-data limits.
- Attribution/cookie/validation/reversal/recurring/final definition.
- Payout threshold/currency/method/tax forms.
- Recordkeeping, audit, incident/complaint notice, terms changes, termination, pending balance và asset removal.

Network umbrella terms không nhất thiết thay vendor/channel agreement. Approved ở network không đồng nghĩa mọi method đều được phép. Một record hết validity, không truy được version áp dụng hoặc có material Change Request chưa duyệt phải chuyển `HOLD`; không dùng last-known terms như thể vẫn còn hiệu lực.

### NO-GO

- Program/site/channel chưa approved khi agreement yêu cầu.
- Không xác định được agreement/policy nào điều khiển, version/effective date hoặc evidence acceptance/approval.
- Không rõ app/utility/redirect/AI-assisted content có được phép.
- Country/KYC/payout không khả dụng.
- Chỉ có thể test bằng self-click/self-purchase bị cấm.
- Muốn thêm arbitrary UTM/sub-ID hoặc mask referrer trái rules.
- Cookie/pixel/postback hoặc data sharing cần thiết cho phương thức dự kiến chưa có privacy/program review.

## 7. Privacy, cookies và personal data

Không có một banner hay câu privacy chung dùng được cho mọi quốc gia. Trước launch, lập [Data/Privacy Inventory](templates/18-data-privacy-inventory.csv) cho:

- Events, cookies, local storage, pixels/tags.
- IP/identifiers/device/approximate location.
- Hosting, analytics, monitoring, consent platform, affiliate/network và processors.
- Purpose, fields, recipients, storage/cross-border transfer.
- Consent/lawful basis review, retention/deletion, user rights và security.

### Không trộn PII với personal data

- `Google Analytics PII` là khái niệm policy/contract của Google: không được gửi dữ liệu Google có thể dùng hoặc nhận ra là định danh cá nhân, gồm PII trong page URL/title, event parameters, campaign parameters hoặc user-entered fields.
- `Personal data` là khái niệm theo luật áp dụng và có thể rộng hơn: IP, cookie/device ID, session/click ID, dữ liệu hành vi và identifier pseudonymous vẫn có thể là personal data.
- Dùng tên `pseudonymous_session_id`, không gọi là anonymous. Mã ngẫu nhiên vẫn có thể là personal data; chỉ gọi anonymous khi đã có bằng chứng anonymization phù hợp và không còn khả năng nối lại theo review áp dụng.
- `Google-prohibited PII = 0` là một gate riêng; nó không thay inventory, data minimization, lawful-basis/consent, retention, rights và security review cho personal data còn lại.

### Thiết kế privacy-first cho MVP

- Không bắt email/account để xem result.
- Không có input free-text nếu không cần.
- Không gửi Google-prohibited PII cho GA4; không đưa direct/sensitive identifiers vào affiliate sub-ID hoặc URLs.
- Session/run/click IDs chỉ dùng khi thật sự cần, random/không chứa định danh trực tiếp, có storage class và retention. Không persist toàn bộ normalized inputs nếu fixture/profile code hoặc aggregate đủ cho audit.
- Server/CDN/security logs có thể tự thu IP, user-agent, URL và IDs; inventory đúng dữ liệu thực tế, giới hạn access/retention và không gọi log là “không có personal data” chỉ vì không có email/tên.
- Data minimization; default settings ít dữ liệu nhất vẫn làm utility hoạt động.
- Consent choice điều khiển tags đúng design; từ chối/rút lại phải có behavior đã test.
- Log/backup/retention/delete có owner.
- Privacy notice mô tả đúng system thật, không copy template rồi để sai.

Google nói chủ site phải hiểu luật theo jurisdiction; consent management gồm lấy lựa chọn, gửi consent signal và bảo đảm tags tôn trọng lựa chọn. Consent Mode không phải cookie banner, không tự xác định lawful basis và không biến một cấu hình thành hợp pháp:

- `Basic`: Google tags bị chặn cho tới khi user tương tác và grant consent; khi user không consent, không gửi dữ liệu cho Google, kể cả consent status.
- `Advanced`: tags load với default consent state; khi consent bị denied, Google vẫn có thể nhận consent state và cookieless pings. Các ping có thể mang functional/coarse information như timestamp, user-agent, referrer và consent state.
- Chỉ chọn mode sau khi review actual requests/data fields, purpose, Google-product agreement/policy và luật áp dụng. Tên “cookieless” không có nghĩa “không xử lý personal data” trong mọi jurisdiction.

Xem [Google consent management](https://support.google.com/analytics/answer/12329599), [Consent Mode](https://support.google.com/analytics/answer/10000067), [GA safeguarding data](https://support.google.com/analytics/answer/6004245) và [PII best practices](https://support.google.com/analytics/answer/6366371).

### Consent test matrix bắt buộc

| Scenario | Phải xác minh và lưu evidence |
|---|---|
| First visit, chưa chọn | Default state đặt trước tag; Basic không gửi Google request, Advanced chỉ gửi đúng denied pings đã duyệt |
| Accept | Chỉ tags/purposes được grant chạy; cookies, requests và parameters đúng inventory |
| Deny | Không có non-essential storage/request ngoài behavior đã được legal/policy review |
| Withdraw/revoke | Tags dừng/đổi state đúng; future requests và storage cleanup đúng thiết kế |
| Return visit | Choice được nhớ trong thời hạn đã duyệt; không reset hoặc tự grant sai |
| Region/market variant | Routing/copy/defaults đúng market; không nhầm EU/EEA, UK, Switzerland và thị trường khác |
| Cookie disabled/ad blocker | Utility vẫn có safe path; event loss được ghi là limitation, không retry/bypass |

Mỗi lần tag, CMP, SDK, consent mode, processor hoặc purpose đổi, tạo Change Request và chạy lại matrix trên production-like preview. Evidence phải gồm configuration version, timestamp, browser/region, network requests, cookies/storage và reviewer; chỉ nhìn banner UI là chưa đủ.

Nếu agreement với Google hoặc Google product đang dùng incorporates [Google EU User Consent Policy](https://www.google.com/about/company/user-consent-policy/), với end users ở EEA, UK và Switzerland phải kiểm riêng các disclosure/consent theo policy, lưu record consent, cung cấp cách revoke và nhận diện các bên thu/nhận/dùng personal data. Đây là lớp policy/contract riêng, không thay luật địa phương.

### Việt Nam

[Luật Bảo vệ dữ liệu cá nhân số 91/2025/QH15](https://congbao.chinhphu.vn/van-ban/luat-so-91-2025-qh15-45578/57730.htm) và [Nghị định 356/2025/NĐ-CP](https://congbao.chinhphu.vn/van-ban/nghi-dinh-so-356-2025-nd-cp-468371.htm) có hiệu lực từ 01/01/2026. Dự án vận hành từ Việt Nam không nên giả định một cookie banner là đủ; cần xác định vai trò xử lý, dữ liệu, chủ thể, quyền, transfer, security, hồ sơ/nghĩa vụ và ngoại lệ áp dụng với người có chuyên môn khi cần.

### EU/EEA và UK

GDPR có thể áp dụng với tổ chức ngoài EU khi cung cấp hàng hóa/dịch vụ hoặc theo dõi hành vi người ở EU. Cookies không strictly necessary thường có yêu cầu consent theo rules áp dụng, nhưng phạm vi/exceptions và luật triển khai phải kiểm theo jurisdiction. Xem [Your Europe — GDPR for business](https://europa.eu/youreurope/business/governance-and-sustainability/digital-and-data-compliance/data-protection-gdpr/index_en.htm) và [Online privacy/cookies](https://europa.eu/youreurope/business/growing/digitalising/online-privacy/index_en.htm). UK có hệ thống riêng và guidance hiện hành về storage/access technologies, consent và exceptions tại [ICO](https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/guidance-on-the-use-of-storage-and-access-technologies/); không nhập nhằng EU và UK.

### California/US privacy

Không phải mọi website nhỏ tự động thuộc CCPA, nhưng nếu đáp ứng phạm vi/threshold hoặc luật khác áp dụng thì có notice và quyền tương ứng. Kiểm [California DOJ CCPA](https://oag.ca.gov/privacy/ccpa) và tư vấn theo facts dự án; không tự suy từ một checklist chung.

## 8. Bảng câu hỏi xác định jurisdiction

Trước mỗi market mới, ghi:

```text
Nơi cá nhân/chủ thể kinh doanh vận hành:
Nơi server/processors lưu/xử lý:
Người dùng được nhắm tới và người dùng thực tế:
Ngôn ngữ/currency/domain/marketing signals:
Dữ liệu/cookie/tag/pixel được dùng:
Có profiling/automated decision/personalization không:
Có email/SMS/minors/sensitive data không:
Có transfer xuyên biên giới không:
Consumer disclosure/advertising rules nào áp dụng:
Business registration/tax/VAT/invoice/KYC nào cần kiểm:
Người review chuyên môn và ngày:
```

Nếu chưa trả lời được, không bật non-essential tracking và không chủ động launch/target market đó. Việc site vẫn có thể được truy cập thụ động từ nơi khác phải được ghi nhận và đánh giá theo facts, signals nhắm mục tiêu, dữ liệu thực tế, luật và platform/program policy áp dụng; câu này không phải yêu cầu geo-block mặc định.

## 9. Privacy notice — nội dung cần mô tả, không phải mẫu pháp lý hoàn chỉnh

- Danh tính/contact của operator/controller theo yêu cầu áp dụng.
- Dữ liệu thu, nguồn, mục đích và căn cứ/consent review.
- Cookies/storage/tags và cách đổi/rút lựa chọn.
- Processors/recipients/affiliate networks.
- Transfer và safeguards nếu áp dụng.
- Retention/deletion.
- Quyền/yêu cầu của người dùng và cách gửi.
- Security/incident contact.
- Automated recommendation: inputs, logic ở mức có ý nghĩa, limits và no high-stakes advice.
- Ngày hiệu lực/changelog.

Privacy notice phải khớp tags/code thật. Mỗi lần thêm SDK/cookie/event/processor, chạy lại inventory và review.

## 10. Security và incident obligations

- MFA, least privilege, separate accounts.
- Secrets ở secret manager/environment, không prompt/repo/log.
- Encryption/access/backup phù hợp; restore test.
- Router allowlist/no open redirect.
- Dependency/plugin update qua preview/QA.
- Logging không chứa secret hoặc Google-prohibited PII; personal/pseudonymous identifiers còn lại phải tối thiểu, có storage class, access control, retention/deletion và privacy review.
- Incident runbook xác định containment, evidence, impact, notification/escalation owner.
- Breach/suspected legal duty → dừng, giữ evidence và hỏi chuyên gia; bot không tự gửi notification pháp lý.

## 11. Checklist trước publish

```text
VALUE
[ ] Original utility/data/method/test; không thin affiliate/spin
[ ] One page one decision job; không mass query variants

TRUTH
[ ] Price/feature/claim official source + checked date
[ ] Không fake experience/review/rating/quote
[ ] Best/comparison có method và limitations

AFFILIATE/LINK
[ ] Program/domain/channel/offer/link active và approved; CTA tra được bằng `link_id + link_version`
[ ] Đúng network + vendor/channel agreement version; validity chưa hết; acceptance/approval evidence tồn tại
[ ] Không material Program/Offer/Link Change Request nào còn mở
[ ] link_type = AFFILIATE/OFFICIAL_NON_AFFILIATE/FALLBACK; delivery_mode = DIRECT/ROUTER, không dùng enum gộp
[ ] Affiliate URL có storage_type đúng và chỉ một public_value/secret_reference; fingerprint không được coi là secret control
[ ] Các cột _json là quoted JSON array hợp lệ hoặc dữ liệu được chuẩn hóa sang child table
[ ] Disclosure rõ, gần CTA, cùng ngôn ngữ, mobile-visible
[ ] link_rel_values_json có sponsored theo project default; ngoại lệ nofollow có lý do + người duyệt
[ ] Không self-click/stuffing/incentive/cloak/UTM/subID trái rule

RIGHTS
[ ] Data collection/source method được phép
[ ] Source/robots/terms/license version và validity còn hiệu lực; không material Change Request mở
[ ] Copy/logo/screenshot/trademark/review asset có quyền

PRIVACY
[ ] Data/tag/cookie/processor inventory current
[ ] Consent/lawful-basis review không chỉ dựa vào CMP/Consent Mode
[ ] Accept/deny/revoke/return-visit/region test matrix pass; network evidence khớp Basic/Advanced design
[ ] Google-prohibited PII trong GA4/event/URL/sub-ID = 0
[ ] Personal/pseudonymous IDs và IP/log data đã inventory, minimize, phân loại storage và retention
[ ] Google EU User Consent Policy được kiểm nếu agreement/product/market làm policy áp dụng
[ ] Privacy notice khớp implementation

CONTROL
[ ] Human fact/compliance review
[ ] Versions, evidence, rollback và incident owner
```

## 12. Refresh cadence

| Tần suất | Kiểm tra |
|---|---|
| Mỗi crawl/hằng ngày | Robots cache ≤24h và fail-closed khi unreachable; job/link/redirect/auth/rate/fraud/security alerts; kill unsafe offer/source |
| Hằng tuần | Sample 5 pages/records/runs; evidence/freshness/disclosure/rel/event; network click reconciliation |
| Hằng tháng | Diff program/network/vendor terms, version/validity/commission/cookie/pixel/postback; source ToS/license; approved domains/channels; pending→final→paid |
| Hằng quý | Data inventory, storage class, retention/deletion/rights, Consent Mode + test matrix, PII/personal-data scan, processors/transfers, country/legal/tax review |
| Trước market/program mới | Full jurisdiction + program gate |
| Khi termination/notice | Pause links/job, remove assets as terms require, preserve pending/final ledger, human review |

## 13. Compliance stop conditions

Dừng ngay phần liên quan nếu:

- Source crawl trái robots/ToS/license, robots decision hết hạn/unreachable nhưng job không fail closed, hoặc phải bypass.
- Program/domain/channel/link không approved, `link_type`/`delivery_mode` sai, URL storage fields mâu thuẫn, record hết validity, sai terms version hoặc material Change Request chưa duyệt.
- Claim/price/experience/review không xác minh.
- Disclosure bị thiếu hoặc paid link thiếu thuộc tính qualifying đã duyệt (`sponsored` mặc định; `nofollow` chỉ khi có ngoại lệ).
- Tracking gửi Google-prohibited PII, xử lý personal data ngoài inventory/review, hoặc chạy trái approved consent/lawful-basis behavior.
- Có self-referral, artificial clicks, cookie stuffing, fake traffic/reviews.
- Merchant/network/cơ quan gửi warning, takedown, suspension hoặc fraud notice.
- Secret/data có thể đã lộ.

Chỉ mở lại sau containment, source/terms review, QA, rollback/fix và human sign-off.

## 14. Source register chính thức đã kiểm

| Chủ đề | Nguồn |
|---|---|
| FTC endorsements/disclosure | [16 CFR 255](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-B/part-255), [FTC FAQ](https://www.ftc.gov/business-guidance/resources/ftcs-endorsement-guides-what-people-are-asking), [Disclosures 101](https://www.ftc.gov/business-guidance/resources/disclosures-101-social-media-influencers) |
| EU/UK affiliate disclosure | [European Commission Influencer Legal Hub](https://commission.europa.eu/topics/consumers/consumer-rights-and-complaints/influencer-legal-hub_en), [ASA/CAP Affiliate Marketing](https://www.asa.org.uk/advice-online/affiliate-marketing.html), [CAP Code Section 2](https://www.asa.org.uk/type/non_broadcast/code_section/02.html) |
| Fake reviews/testimonials | [16 CFR 465](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-D/part-465), [FTC Q&A](https://www.ftc.gov/business-guidance/resources/consumer-reviews-testimonials-rule-questions-answers) |
| Google AI/scaled/thin content | [Spam policies](https://developers.google.com/search/docs/essentials/spam-policies), [GenAI guidance](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content), [People-first](https://developers.google.com/search/docs/fundamentals/creating-helpful-content), [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) |
| Affiliate link qualification | [Qualify outbound links](https://developers.google.com/search/docs/crawling-indexing/qualify-outbound-links) |
| Robots protocol | [RFC 9309](https://www.rfc-editor.org/rfc/rfc9309.html) |
| Search Console | [Getting started](https://support.google.com/webmasters/answer/10267942), [Sitemaps](https://support.google.com/webmasters/answer/7451001), [Page indexing](https://support.google.com/webmasters/answer/7440203), [URL Inspection](https://support.google.com/webmasters/answer/12482179), [Impressions/clicks/position](https://support.google.com/webmasters/answer/7042828) |
| GA events/click tracking | [GA4 setup](https://support.google.com/analytics/answer/14183469), [GA4 events](https://developers.google.com/analytics/devguides/collection/ga4/events), [Outbound click tutorial](https://support.google.com/analytics/answer/13566436), [DebugView](https://support.google.com/analytics/answer/7201382), [Search Console link](https://support.google.com/analytics/answer/10737381) |
| GA privacy/PII | [Safeguarding data](https://support.google.com/analytics/answer/6004245), [Avoid sending PII](https://support.google.com/analytics/answer/6366371), [Data collection](https://support.google.com/analytics/answer/11593727) |
| Consent/Google policy | [Consent management](https://support.google.com/analytics/answer/12329599), [Consent Mode](https://support.google.com/analytics/answer/10000067), [Google EU User Consent Policy](https://www.google.com/about/company/user-consent-policy/) |
| Affiliate-program examples, không phải rule chung | [HubSpot Affiliate Agreement](https://legal.hubspot.com/affiliate-program-agreement), [HubSpot Program Policies](https://www.hubspot.com/partners/affiliates/program-policies), [PartnerStack Partner Agreement](https://partnerstack.com/legal/partner-agreement) |
| Việt Nam personal data | [Luật 91/2025/QH15](https://congbao.chinhphu.vn/van-ban/luat-so-91-2025-qh15-45578/57730.htm), [Nghị định 356/2025/NĐ-CP](https://congbao.chinhphu.vn/van-ban/nghi-dinh-so-356-2025-nd-cp-468371.htm) |
| EU privacy/cookies | [GDPR for business](https://europa.eu/youreurope/business/governance-and-sustainability/digital-and-data-compliance/data-protection-gdpr/index_en.htm), [Online privacy](https://europa.eu/youreurope/business/growing/digitalising/online-privacy/index_en.htm) |
| UK storage/access technologies | [ICO guidance](https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/guidance-on-the-use-of-storage-and-access-technologies/) |
| California privacy | [California DOJ CCPA](https://oag.ca.gov/privacy/ccpa) |

Source register này là điểm bắt đầu; agreement/policy của chính affiliate program và facts/jurisdictions của dự án vẫn phải được rà riêng.

