# 05 — SOP thực thi

## Cách đọc một SOP

Mỗi SOP có:

- **Owner:** người chịu trách nhiệm cuối; bot không phải owner pháp lý.
- **Trigger:** khi nào phải chạy.
- **Input:** thứ phải có trước khi bắt đầu.
- **Các bước:** làm theo đúng thứ tự.
- **Output/Evidence:** thứ chứng minh đã làm.
- **QA/Pass:** điều kiện kết thúc thành công.
- **Escalate/Rollback:** lúc dừng và chuyển người xử lý.

Quy ước severity:

- `SEV-1/P0`: rò secret/dữ liệu, redirect nguy hiểm/sai diện rộng, gian lận, tiền bị nhân đôi, vi phạm điều khoản — dừng ngay.
- `SEV-2/P1`: utility/tracking quan trọng không hoạt động, recommendation sai hard constraint, nhiều trang sai — cấm publish/launch.
- `SEV-3/P2`: lỗi giới hạn, có workaround — sửa có thời hạn.
- `SEV-4/P3`: trình bày/cosmetic — backlog.

Ranh giới quyền giữa người và bot:

| Bot được làm trong policy đã duyệt | Con người phải quyết định hoặc phê duyệt |
|---|---|
| Đọc nguồn đã allowlist; chuẩn hóa; chạy schema/duplicate/freshness checks; tạo diff và change queue | Quyền truy cập/tái sử dụng nguồn, affiliate terms, privacy/legal/IP và chương trình `GO / HOLD / NO-GO` |
| Chạy scoring deterministic, Golden Tests, tạo draft/explanation từ evidence IDs | Hard constraints, scoring material, disclosure, claim, trang/release được public |
| Ghi event/import append-only; tính derived views/dashboard; cảnh báo exception | Xác nhận sao kê/tiền nhận, xử lý discrepancy material, credential/KYC/tax và quyết định scale/kill |
| Tự áp dụng đúng allowlist low-risk sau khi đạt gate L3 | Mở rộng allowlist/quyền tự động, rollback rủi ro cao và mọi ngoại lệ chưa có policy |

Bot phải dừng ở trạng thái an toàn và chuyển người xử lý khi thiếu evidence, vượt budget/rate limit, gặp terms mơ hồ, P0/P1 hoặc thay đổi ngoài allowlist.

## SOP-01 — Thẩm định affiliate program

**Owner:** Business owner.  
**Trigger:** khi cân nhắc một merchant/network mới; rà lại hàng tháng hoặc khi có notice/terms change.  
**Input:** official program page, agreement/policies, target country, planned channels, payout needs.

### Các bước

1. Xác minh URL thuộc merchant/network chính thức và program còn nhận application.
2. Lưu agreement URL, policy URLs, `checked_at`, version/last-modified nếu có và evidence reference.
3. Kiểm eligibility: quốc gia, loại publisher, website status, age/business requirement.
4. Kiểm channel: SEO/content, tool/app, email, social/video, PPC/direct linking, coupon/deal, incentives.
5. Ghi điều cấm: brand bidding, self/client/employee referral, cookie stuffing, forced/hidden redirect, referrer masking, fake lead/review, automated traffic.
6. Ghi link rules: approved domains, deep link, redirect/cloaking, `subid/clickref`, UTM, API/feed, cache.
7. Ghi quyền IP: name/logo/screenshot/copy/price/rating/review/trademark.
8. Ghi economics: commission base/rate/type, attribution/cookie window, validation, reversal/clawback, recurring duration.
9. Ghi payout: threshold, schedule, currency, method, KYC/tax form, termination effect.
10. Tạo danh sách câu hỏi `UNKNOWN/BLOCKED`; không tự suy diễn.
11. Con người quyết định `GO / HOLD / NO-GO`, ký và đặt review date.

**Output/Evidence:** [Program Review](templates/04-affiliate-program-review.md), decision record, saved official links.  
**QA/Pass:** tất cả trường critical có nguồn; target domain/channel và payout thực sự khả dụng; không blocker chưa giải.  
**Escalate:** terms mâu thuẫn/mơ hồ, không biết app/utility/AI content được phép, không nhận payout, hoặc cần diễn giải pháp lý.  
**Rollback:** pause các `link_id/link_version` và offer liên quan, gỡ/đổi CTA theo policy, giữ historical ledger và làm theo termination requirements.

## SOP-02 — Đăng ký nguồn dữ liệu

**Owner:** Data owner.  
**Trigger:** trước khi bot hoặc người dùng một nguồn mới cho production.  
**Input:** source URL/API, intended fields, purpose, access method.

### Các bước

1. Xác định owner/publisher và phân loại `OFFICIAL_PAGE / API / FEED / EXPORT / EMAIL / SECONDARY / COMMUNITY`.
2. Kiểm `robots.txt` nếu crawl; `Disallow` cho user-agent là stop condition.
3. Đọc ToS/license/API docs; ghi automated access, commercial reuse, cache, attribution, rate limits.
4. Kiểm quyền dùng fact, text, screenshot, logo, review/rating và database compilation riêng.
5. Xác nhận không bypass login/paywall/CAPTCHA/control.
6. Chọn fields tối thiểu, rate limit thấp, timeout, backoff và stop on `401/403/429`.
7. Đặt source tier, SLA, `next_check_at`, owner và kill switch.
8. Lưu một evidence sample hợp pháp và mô tả cách trích xuất.
9. Con người duyệt `ACTIVE / MANUAL_ONLY / HOLD / REJECTED`.

**Output/Evidence:** Source Registry record.  
**QA/Pass:** quyền và phạm vi rõ; access test trong giới hạn; owner/SLA/kill switch có thật.  
**Escalate:** quyền tái sử dụng mơ hồ, terms cấm, CAPTCHA/login, PII hoặc nội dung IP-sensitive.  
**Rollback:** stop source job, quarantine records chưa duyệt, chuyển record phụ thuộc sang `STALE/BLOCKED`.

## SOP-03 — Thu thập product/plan/feature

**Owner:** Data owner.  
**Trigger:** thêm sản phẩm hoặc đến `next_check_at`.  
**Input:** approved Source Registry, Product ID, schema/data dictionary.

### Các bước

1. Lấy name/vendor/category/status/official URL/region.
2. Với từng plan, ghi currency, billing interval, actual charge, annual-equivalent nếu cần; không trộn hai khái niệm.
3. Ghi tax/VAT inclusion chỉ khi nguồn nói rõ; nếu không, `UNKNOWN` + assumption ở UI.
4. Ghi trial/refund/quota/limits và critical features.
5. Ghi limitation/missing feature có ảnh hưởng hard constraint.
6. Với mỗi critical field, tạo evidence: source ID, source URL, summary, checked/effective dates, checker, confidence.
7. Giữ raw reference/snapshot theo quyền cho phép; không copy nguyên mô tả dài.
8. Đưa record mới vào staging, chưa public.

**Output/Evidence:** Product, Plan, Feature, Evidence records.  
**QA/Pass:** schema valid; 100% critical fields có evidence hoặc `UNKNOWN`; region/currency/billing rõ.  
**Escalate:** price theo locale/login, source mâu thuẫn, plan biến mất, must-have claim không còn nguồn.  
**Rollback:** giữ production version, đánh staging change `REJECTED/BLOCKED`.

## SOP-04 — Chuẩn hóa và QA dữ liệu

**Owner:** Data owner; QA reviewer độc lập với người nhập khi có thể.  
**Trigger:** sau mỗi import/edit và trước publish.  
**Input:** raw/staging records, schema, normalization rules.

### Các bước

1. Chuẩn hóa ID, enum, currency code, date/time UTC, unit và boolean/null.
2. Chạy required-field/schema/range/referential-integrity checks.
3. Phát hiện duplicate theo canonical product/plan/vendor + region.
4. So sánh với production và tạo diff.
5. Tính bốn thành phần confidence 0–100: completeness, freshness, source authority, consistency.
6. Tính gợi ý:

```text
Confidence =
0,40 × Completeness
+ 0,30 × Freshness
+ 0,20 × Source Authority
+ 0,10 × Consistency
```

   Trong đó phải có data dictionary và formula version:

   - `Completeness = required fields có evidence / required fields`.
   - `Freshness = weighted fields còn trong SLA / weighted fields`.
   - `Source Authority` lấy từ lookup Source Tier đã duyệt, không chấm cảm tính mỗi lần.
   - `Consistency` lấy từ conflict/locale/cross-source rules đã viết trước.

7. Lưu cả bốn component + `confidence_formula_version`; không chỉ lưu điểm tổng.
8. Không dùng score để che critical missing/conflict: một field hard constraint bị conflict vẫn là blocker dù confidence trung bình cao.
9. Đưa records đạt sang approved staging; records khác vào exception queue.

**Output/Evidence:** validated dataset, QA report, exception list, diff.  
**QA/Pass:** schema 100%; public critical evidence 100%; duplicate 0; conflict critical 0.  
**Escalate:** material diff, confidence <90 cho product đề xuất, critical missing/conflict.  
**Rollback:** bỏ staging batch bằng batch ID; production không đổi.

## SOP-05 — Xử lý thay đổi và xung đột

**Owner:** Data owner; business owner duyệt material changes.  
**Trigger:** detected diff hoặc hai nguồn/locale cho giá trị khác.  
**Input:** old/new value, sources, impacted product/pages/runs.

### Phân loại

- `COSMETIC`: spelling/layout, không đổi ý nghĩa.
- `MINOR`: mô tả rõ hơn, không đổi eligibility/cost/score.
- `MATERIAL`: price, plan, region, must-have feature, ranking, affiliate terms/link/status.
- `CRITICAL`: misleading recommendation, widespread wrong data, unsafe link, policy breach.

### Các bước

1. Freeze auto-apply cho field/record liên quan.
2. Ghi before/after, `effective_at`, `detected_at`, `verified_at`, source/version.
3. Re-check official source và locale; nguồn cộng đồng không thắng nguồn chính thức.
4. Nếu vẫn mâu thuẫn, đặt `CONFLICT`; ẩn claim hoặc hiển thị chưa xác minh theo approved UX.
5. Tính impact: pages, Golden Tests, current top results, offers, content claims.
6. Cosmetic/minor chỉ auto-apply nếu policy đã duyệt và đủ supervised runs.
7. Material/critical cần human approval, full tests và release note.

**Output/Evidence:** Change Request, impact report, approval/rejection.  
**QA/Pass:** provenance + diff + impact + rollback có đủ; tests pass.  
**Escalate:** critical change, legal/IP terms, top ranking đổi mạnh, price tăng/giảm bất thường.  
**Rollback:** restore last-known-good data version; invalidate cache; re-run impacted pages.

## SOP-06 — Tính recommendation và User Fit Score

**Owner:** Product owner.  
**Trigger:** utility submission hoặc scoring change test.  
**Input:** validated data snapshot, input schema, scoring rules/version.

### Các bước

1. Validate input; không log free text/PII không cần thiết.
2. Tạo `journey_id` tiền tố `JNY-` và `recommendation_run_id` tiền tố `REC-`; lưu input tối thiểu đã chuẩn hóa hoặc snapshot/hash phù hợp retention, cùng data/scoring/input-schema versions.
3. Áp hard filters: market/region, budget hard cap, must-have, product active, evidence not blocked.
4. Chấm 0–5 theo User Fit criteria; tính weighted score.
5. Tính total cost với cùng region/currency/billing assumptions.
6. Tạo `Best Fit`, `Cheapest qualified`, `Alternative`; cho phép non-affiliate product đứng đầu.
7. Tạo explanation từ fact IDs/rules. Ưu tiên reason template deterministic; nếu LLM chỉ diễn đạt, lưu template/prompt/model version, rendered output/hash và không để LLM tự thêm claim.
8. Hiển thị trade-offs, limitations, data date, confidence và assumption.
9. Nếu không đủ candidate/confidence, trả no-result trung thực; không lấp bằng sản phẩm kém phù hợp.

**Output/Evidence:** reproducible Recommendation Run.  
**QA/Pass:** cùng input + versions tạo cùng ranking/cost/reason IDs; prose LLM nếu có được audit bằng stored output/version; hard tests 100%; price/why trace được; commission không trong User Fit.  
**Escalate:** no candidate spike, confidence thấp, result vượt budget, top results đổi sau data/scoring update.  
**Rollback:** pin previous scoring/data version, disable affected rule, show maintenance/no-result nếu cần.

## SOP-07 — Tạo và xuất bản content/utility page

**Owner:** Editorial owner; business owner duyệt trang rủi ro.  
**Trigger:** approved Page Brief hoặc material update.  
**Input:** page intent, evidence set, approved data/scoring version, disclosure copy, Link Registry.

### Các bước

1. Xác nhận page giải quyết một decision job riêng và có original value.
2. Tạo outline: answer → method → evidence/comparison → trade-off → utility → CTA.
3. Draft chỉ dùng approved evidence; mọi fact gắn evidence ID.
4. Không viết experience/test/review ở ngôi cá nhân nếu không có evidence thật.
5. Đặt disclosure rõ trước/gần recommendation và affiliate CTA đầu; cùng ngôn ngữ trang.
6. Link affiliate từ Link Registry và gắn qualifying `rel` đã duyệt; chuẩn nội bộ mặc định là `rel="sponsored"`.
7. Hiển thị currency/billing/region assumptions, last checked và limitations.
8. Chạy fact, originality, mobile, keyboard, link, event, no-result, SEO metadata QA.
9. Lưu page/data/scoring versions và rollback target.
10. Reviewer chọn `PUBLISH / HOLD / REJECT`; production chỉ đổi sau approval.

**Output/Evidence:** public URL, [Publishing Approval](templates/09-publishing-approval.md), release log.  
**QA/Pass:** Definition of Done trong checklist đạt 100% critical items, 0 P0/P1.  
**Escalate:** unsupported claim, hidden disclosure, fake review, stale price, policy/rights ambiguity.  
**Rollback:** unpublish/noindex hoặc restore prior page; pause affected `link_id/link_version` và offer nếu có; preserve incident evidence.

## SOP-08 — Quản lý product link, affiliate link và optional router

**Owner:** Affiliate operations owner.  
**Trigger:** program approved, offer/link changed, health alert.  
**Input:** approved Program ID nếu monetized, official affiliate/non-affiliate URL, intended destination, approved domain/channel và program rules.

### Các bước

1. Tạo stable `link_id` tiền tố `LNK-` và immutable `link_version`; nối tới `offer_id/offer_version` nếu có chương trình thương mại. Link record lưu merchant/product/program, `link_type = AFFILIATE / OFFICIAL_NON_AFFILIATE / FALLBACK`, intended destination, approved channel/domain và effective dates. Offer và link là hai grain khác nhau: một offer có thể có nhiều link/version theo placement hoặc destination.
2. Chọn `delivery_mode` đã duyệt:
   - `DIRECT`: anchor đi thẳng tới official affiliate/non-affiliate URL. Đây là mặc định nếu program không cần hoặc không cho router.
   - `ROUTER`: dùng `/go/{link_id}` để lookup đúng active `link_version`, chỉ khi program cho phép và lợi ích tracking/kiểm soát đáng để vận hành.
3. Validate scheme, destination domain, affiliate ID/link format và expiry mà không tạo artificial network click.
4. Dùng một custom event `product_outbound_click` cho mọi product link. `affiliate_click` là metric **suy ra**, không phải event thứ hai: `link_type = AFFILIATE` và `link_id/link_version` active tại thời điểm click, bất kể `delivery_mode`.
5. Tạo một `outbound_click_id` tiền tố `CLK-` cho cả hai mode. Với direct link, browser tạo ID và ghi event theo kiểu best-effort nhưng không được giữ navigation để chờ analytics. Với router, chuyển cùng ID tới server; server chỉ tạo ID nếu client chưa có, ghi append-only event tối thiểu rồi redirect nhanh. Browser/server record dùng cùng ID để dedupe, không tính thành hai click.
6. Chỉ gắn `subid/clickref = outbound_click_id` khi terms cho phép. Không truyền PII, session profile hoặc free text.
7. Dùng link-version status `ACTIVE / PAUSED / EXPIRED / BLOCKED`; link inactive dùng approved official non-affiliate fallback hoặc remove CTA theo policy. Offer/program status vẫn được quản lý riêng.
8. Affiliate anchor có qualifying `rel` đã duyệt; chuẩn nội bộ mặc định là `rel="sponsored"` và ngoại lệ `nofollow` phải có lý do. Disclosure vẫn bắt buộc. Non-affiliate link không bị gắn giả là affiliate.
9. Monitor destination bằng validator/API/test mode hoặc phương thức được phép; không tự mở live affiliate link lặp lại.
10. Soft launch chưa monetized phải ghi `reconciliation_status = NOT_APPLICABLE_NOT_MONETIZED`, có test plan cho ngày program được approved và không tạo số click/order giả.

**Output/Evidence:** Link Registry có `link_id/link_version` và optional offer reference, [Event/Click Log](templates/27-event-click-log.csv), link health record và reconciliation/test plan.  
**QA/Pass:** đúng merchant/product/destination; affiliate derivation không phát event kép; direct link và optional router đều pass; router không open redirect; inactive fallback an toàn.  
**Escalate:** domain lạ, link hijack, fraud warning, spike, program termination hoặc rule không rõ.  
**Rollback:** pause đúng `link_id/link_version` và Offer version liên quan, thay bằng approved official non-affiliate fallback hoặc remove CTA, re-QA affected pages; giữ historical click events.

## SOP-09 — Cài đặt và QA tracking

**Owner:** Analytics owner; privacy owner duyệt data/consent.  
**Trigger:** thêm/sửa event, tag, router, consent hoặc release.  
**Input:** Measurement Plan/Event Contract, site preview, privacy decision, network capability.

### Các bước

1. Ghi event name, exact trigger, required/optional params, data type, retention, purpose và owner.
2. Dùng `journey_id`, `pseudonymous_session_id`, recommendation run, event và outbound click IDs; không gửi name, email, phone, free-text hoặc sensitive input cho analytics.
3. Cài events: `utility_start`, `utility_complete`, `result_view`, `comparison_open`, `product_outbound_click`, `feedback_submit`, `data_error_shown`. Không phát thêm một `affiliate_click` browser event; suy ra affiliate click từ active `link_id/link_version` có `link_type=AFFILIATE`.
4. Ghi append-only vào [Event/Click Log](templates/27-event-click-log.csv); event-specific params đã privacy-filtered nằm trong JSON đúng Event Contract và mỗi record có schema validation status. Nếu dùng router, browser event và server event phải có source/grain/dedupe rule rõ; không cộng hai bản ghi như hai user clicks.
5. Cấu hình consent/tag behavior theo quyết định đã review cho target market; banner không tự động làm hệ thống compliant.
6. Test từng action một lần trong debug/realtime; kiểm required fields, timestamp, duplication và test flag.
7. Test no-consent/consent-revoked/cookie-disabled/ad-blocker scenarios phù hợp.
8. Nếu network có sandbox/test mode hoặc validator, dùng cơ chế đó; không self-purchase/artificial click.
9. Với soft launch non-affiliate, QA đến `product_outbound_click`; phần network/order/commission là `NOT_APPLICABLE_NOT_MONETIZED` kèm test plan, không phải zero conversion.
10. Lưu expected vs observed, event/log batch references và ký QA.

**Output/Evidence:** [Event Contract](templates/21-event-contract.csv), [Event/Click Log](templates/27-event-click-log.csv), Tracking QA report/screens/log references.  
**QA/Pass:** synthetic/test required fields = 100%; duplicate = 0; Google-prohibited PII/direct identifiers = 0; personal-data inventory/retention/consent-or-basis review pass; consent behavior đúng design. Production percentage chỉ đánh giá khi đạt minimum sample ghi trong Event Contract.  
**Escalate:** PII, event loss/duplicate, consent mismatch, network warning, inability to map IDs.  
**Rollback:** disable new tag/event, restore prior tag version, preserve raw logs, mark affected window.

## SOP-10 — Chạy bot/job định kỳ

**Owner:** Operations owner.  
**Trigger:** scheduler hoặc manual approved run.  
**Input:** job version, source/link/offer allowlist phù hợp task, budget/rate limits, last run state.

### Các bước

1. Pre-flight: permissions active, source SLA, credential available, budget/rate limit còn đủ, kill switch off.
2. Tạo `job_run_id` tiền tố `JOB-`, ghi version/start time/input cursor; không dùng cùng ID namespace với `recommendation_run_id` tiền tố `REC-`.
3. Fetch trong phạm vi duyệt. `401/403` hoặc terms signal dừng ngay; `429` chỉ retry theo `Retry-After`/bounded backoff đã duyệt rồi pause + alert nếu vẫn lỗi.
4. Validate/normalize; invalid records vào exception queue, không tự bỏ qua.
5. Diff với current; material/critical changes chờ approval.
6. Apply chỉ low-risk changes được policy cho phép; dùng idempotency key.
7. Verify output counts/checksums/samples.
8. Ghi created/updated/unchanged/rejected/conflict/errors/retries/cost.
9. Alert theo severity; retry có số lần tối đa, không vòng lặp vô hạn.
10. Kết thúc `SUCCESS / PARTIAL / FAILED`; không gọi success nếu critical substep fail.

**Output/Evidence:** [Daily Run Log](templates/12-daily-run-log.md), exception queue, alert references.  
**QA/Pass:** start/end/status/counts có đủ; rerun không duplicate; cost/rate trong trần.  
**Escalate:** fail sau retry budget, unexpected data collapse/spike, `401/403`, `429` lặp lại, secret in log, material diff.  
**Rollback:** stop job, revert batch/job run ID, restore cursor/checkpoint an toàn, manual mode.

## SOP-11 — Import order/conversion và chống trùng

**Owner:** Revenue operations owner.  
**Trigger:** webhook/API/CSV/export theo lịch.  
**Input:** network-authenticated raw payload/export, mapping spec, previous cursor/batch.

### Các bước

1. Tạo `import_batch_id`; lưu raw reference và integrity metadata theo quyền/retention.
2. Verify source/auth/signature/file identity; rejected payload không normalize.
3. Dùng [Network Status Mapping](templates/28-network-status-mapping.csv) đã được duyệt để map original status sang canonical status; không tự suy tên trạng thái.
4. Map network/program/source transaction ID/external order ID/line ID/type/timestamps/amount/currency/status. Mỗi observation/status import là một `order_event_id` append-only trong [Orders](templates/14a-orders.csv); `order_amount` là snapshot của observation, không được cộng tất cả observations như doanh thu mới.
5. Deduplicate ưu tiên immutable network transaction/source record ID. Nếu network không có, dùng natural key được Program Review phê duyệt; không mặc định một key dùng cho mọi network.
6. Ghép click chỉ bằng sub-ID/clickref/outbound ID được network trả. Nếu không có, dùng `UNMATCHED`; không gọi suy đoán theo ngày là exact.
7. Re-run same batch để kiểm idempotency; cùng source record không sinh order event mới.
8. Đối chiếu raw count/sum với normalized/rejected/duplicate counts theo cùng currency và record grain.
9. Nếu soft launch dùng non-affiliate links, ghi import/reconciliation `NOT_APPLICABLE_NOT_MONETIZED` và giữ test plan; không tạo order giả để làm dashboard đủ cột.

**Output/Evidence:** [Orders](templates/14a-orders.csv), Import Report, Network Status Mapping, unmatched/exception queue.  
**QA/Pass:** rerun tăng 0 order events; raw/normalized counts reconcile; invalid transitions blocked; original status vẫn truy lại được.  
**Escalate:** spike/negative amount/new currency, duplicate, signature fail, unmatched tăng mạnh.  
**Rollback:** quarantine batch, loại batch khỏi current derived view bằng compensating record; không xóa append-only order events/raw evidence.

## SOP-12 — Theo dõi valid/final/reversal commission

**Owner:** Revenue operations owner.  
**Trigger:** network status update/export/API.  
**Input:** Order Ledger, network-reported status and commission.

### Các bước

1. Map network-specific states sang canonical states nhưng giữ original state.
2. Commission state graph gồm `REPORTED / PENDING / VALID / REJECTED / FINAL / REVERSED / CLAWBACK`; network được phép bỏ qua một bước chỉ khi raw status + mapping chứng minh. `PAID` là payout/allocation state riêng, không là commission transition.
3. Append mỗi thay đổi vào [Commission Transitions](templates/14b-commission-transitions.csv) với `from/to`, original status, amount, effective/reported/import timestamps, mapping version và reason. Ghi rõ `amount_semantics = SNAPSHOT / DELTA`; derived current view chọn transition hiệu lực mới nhất cho `SNAPSHOT` hoặc cộng signed deltas cho `DELTA`, tuyệt đối không cộng lặp mọi snapshot.
4. Chỉ gắn `FINAL` khi network nói final/locked/approved theo định nghĩa program đã lưu; không suy ra chỉ vì order già.
5. Giữ amount/currency gốc; conversion sang reporting currency lưu FX source/rate/date riêng ở layer báo cáo.
6. Reconcile count/amount theo cùng program, entity grain và cohort; recurring commissions tách khỏi initial order conversion khi tính rate.
7. Lưu riêng `validation_maturity_at`, `finalization_maturity_at`, `expected_payout_at`; không dùng một maturity flag chung.

**Output/Evidence:** [Commission Transitions](templates/14b-commission-transitions.csv), status mapping/history, aging/reversal report.  
**QA/Pass:** totals khớp network; state transitions append-only và trace được; Ordered/Valid/Final tách; payment không ghi đè commission status.  
**Escalate:** state đi lùi không có reversal, final amount đổi, rejection/reversal spike, missing commission.  
**Rollback:** correct bằng compensating status record, không xóa lịch sử; re-run reconciliation.

## SOP-13 — Đối soát payout/payment

**Owner:** Business/finance owner; bot chỉ hỗ trợ tính.  
**Trigger:** payout report hoặc tiền thực nhận.  
**Input:** final commissions due, payout statement, bank/payment evidence.

### Các bước

1. Nhập payout ID, record version, period, due date, grace days, gross, fee, net, currency, sent/received dates vào [Payouts](templates/14c-payouts.csv). Khi statement/receipt đổi, append version mới trỏ version cũ; không ghi đè bằng chứng.
2. Map payout tới commissions bằng [Payout Allocations](templates/14d-payout-allocations.csv); một payout có nhiều commission và một commission có thể có nhiều allocations. Giữ unmatched lines riêng. Correction dùng record `REVERSE` âm trỏ allocation cũ rồi append allocation mới; không sửa allocation đã ghi.
3. So sánh ba basis riêng: `final gross due`, `statement gross/net`, `bank/payment net received`. Không chia net cho gross rồi gọi là coverage; tính difference/materiality riêng cho gross allocation và net receipt.
4. Ghi FX rate/source/time, known fees và reporting currency; không đổi số gốc.
5. Người có quyền kiểm tra sao kê và xác nhận payout received; bot không truy cập/hiển thị credential.
6. Mở exception nếu:
   - `abs(difference) > max(materiality_absolute_by_currency, expected × materiality_percent)`;
   - expected = 0 nhưng có amount không giải thích được; hoặc
   - chưa nhận sau `payout_due_at + grace_days`.
7. Sau khi reconcile, khóa kỳ báo cáo; correction dùng adjustment/allocation record, không sửa statement cũ.

**Output/Evidence:** Payouts, Payout Allocations, reconciliation report, payment evidence reference.  
**QA/Pass:** mỗi received payout có bằng chứng tiền nhận; gross coverage và net cash realization có cùng basis/currency; allocation totals khớp statement.  
**Escalate:** vượt materiality cấu hình, quá due + grace, unmatched allocation, payout account/KYC issue.  
**Rollback:** không xóa/đổi bank statement; reverse mapping bằng adjustment và reopen period có phê duyệt.

## SOP-14 — Refresh trang và dữ liệu định kỳ

**Owner:** Editorial + Data owner.  
**Trigger:** stale alert, material change, user report, high-value page review date.  
**Input:** affected page list, change requests, current evidence/data/scoring versions.

### Các bước

1. Ưu tiên theo user impact, traffic/click/final revenue và severity; trust risk luôn cao hơn traffic.
2. Re-verify every critical field dùng trên trang; không chỉ đổi last-updated date.
3. Resolve conflicts/unknown hoặc ẩn claim.
4. Re-run impacted Golden Tests và recommendation fixtures.
5. Update content/explanation/price/limitations/disclosure/link as needed.
6. Chạy full pre-publish QA.
7. Publish approved version, invalidate caches, verify live.
8. Ghi release/change log và next review.

**Output/Evidence:** refreshed page/data, release log, QA evidence.  
**QA/Pass:** critical evidence 100%, tests pass, link/event/disclosure pass.  
**Escalate:** product shutdown, program termination, top result change, mass staleness, rights change.  
**Rollback:** restore prior safe page/data; nếu prior cũng sai thì unpublish/deactivate affected recommendation.

## SOP-15 — Thiết kế và chạy thử nghiệm

**Owner:** Product/analytics owner.  
**Trigger:** một vấn đề đo được đã có baseline và data quality pass.  
**Input:** experiment card, baseline, sample/time estimate, implementation plan.

### Các bước

1. Viết một hypothesis theo dạng “Nếu… thì… vì…”.
2. Chọn một primary metric và tối thiểu một guardrail: misleading/no-result/error/disclosure/complaint.
3. Chọn population/cohort; viết exclusions và maturity rule.
4. Đặt minimum time/sample hoặc quy tắc `INCONCLUSIVE`; không sửa sau khi thấy kết quả trừ safety stop.
5. Chỉ thay một biến/nhóm biến có thể diễn giải.
6. QA variant parity: data, tracking, disclosure, speed, assignment.
7. Chạy; monitor guardrails, không “peeking” rồi kết luận sớm.
8. Phân tích `KEEP / REJECT / INCONCLUSIVE`; ghi limitation và follow-up.

**Output/Evidence:** [Experiment Card](templates/10-experiment-card.md), result report, decision log.  
**QA/Pass:** tracking balanced; decision dùng matured cohort; pending commission không là final result.  
**Escalate:** disclosure missing, user harm/complaints, tracking mismatch, unbalanced allocation.  
**Rollback:** stop experiment, route 100% về control/last-good, mark affected data window.

## SOP-16 — Xử lý sự cố và rollback

**Owner:** Incident commander là con người được chỉ định.  
**Trigger:** alert hoặc report có user/data/money/policy impact.  
**Input:** incident signal, run/release/data/link versions, logs.

### Các bước

1. Tạo Incident ID, timestamp, reporter; phân severity.
2. Contain: kill switch đúng scope; nếu chưa rõ scope, ưu tiên chặn unsafe redirect/claim/job.
3. Bảo toàn logs/evidence; không xóa hoặc “dọn” trước điều tra.
4. Xác định last-known-good và rollback.
5. Verify containment trên live, mobile và affected routes.
6. Xác định impact window/users/pages/data/orders/money/policy.
7. Fix ở staging; test regression/restore/reconciliation.
8. Human approve reopening; monitor tăng cường.
9. Trong 3 ngày làm postmortem: timeline, root cause, detection gap, corrective/preventive actions, owners/dates.

**Output/Evidence:** [Incident Report](templates/13-incident-report.md), timeline, fix/rollback proof, postmortem.  
**QA/Pass:** service safe, ledger reconciled, preventive action tracked.  
**Escalate ngoài nhóm:** suspected breach, legal/IP complaint, affiliate/network enforcement, financial discrepancy, notification duty.  
**Rollback:** là bước bắt buộc của containment nếu current version unsafe; không chờ root cause hoàn chỉnh.

## SOP-17 — Weekly review và quyết định portfolio

**Owner:** Business owner.  
**Trigger:** cùng một ngày mỗi tuần; formal gates Day 7/21/28/35/42/49/63/70/84/90.  
**Input:** dashboard, data quality, incidents, costs/time, experiments, partner status.

### Các bước

1. Kiểm data window, timezone, internal traffic, sample và cohort maturity.
2. Review theo tầng: Delivery → Trust/Data → Discovery → Utility Funnel → Affiliate Funnel → Money → Operations.
3. Ghi `FACT / INFERENCE / UNKNOWN` riêng.
4. Kiểm guardrails trước metrics tăng trưởng.
5. Chọn tối đa 3 vấn đề quan trọng; mỗi vấn đề có root-cause hypothesis và next evidence.
6. Đánh trạng thái asset: `TEST / WATCH / MAINTAIN / SCALE-CANDIDATE / HOLD / KILL`.
7. Chạy gate thích hợp; dùng `NOT_ENOUGH_DATA` nếu chưa đủ mẫu.
8. Ký decision, budget/scope, owner và review date.

**Output/Evidence:** [Weekly Review](templates/11-weekly-review.md), Decision Log, prioritized backlog.  
**QA/Pass:** không kết luận từ pageviews/pending money riêng; guardrails pass; decision trace được.  
**Escalate:** any P0/P1, legal/terms ambiguity, budget breach, decision to scale/open new niche.  
**Rollback:** reverse experiment/scope/budget change theo decision record.

## Điều kiện nâng quyền tự động hóa

### Từ L1 sang L2 — job chạy và tạo change queue

- [ ] Manual SOP đã chạy đúng ít nhất 3 lần.
- [ ] Input/output/error states được viết rõ.
- [ ] Scheduler, timeout, retry, budget/rate limit và owner có thật.
- [ ] Job không thay production trước human approval.
- [ ] Hai acceptance runs + failure drill pass.

### Từ L2 sang L3 giới hạn — tự áp dụng low-risk change

- [ ] Chỉ đúng một task type đảo ngược được.
- [ ] Tối thiểu 20 supervised runs **khác input/condition hợp lý** và trải qua ít nhất 14 ngày lịch hoặc một chu kỳ SLA thật; không chạy lặp cùng input chỉ để đủ số.
- [ ] Có no-change, valid change, transient failure và material-change-blocked cases.
- [ ] 0 critical error; synthetic acceptance checks = 100%; production QA rate chỉ dùng khi có denominator/minimum sample đã định nghĩa.
- [ ] Alert, kill switch, rollback và audit log đã test.
- [ ] Explicit allowlist cho fields/actions; price, hard constraints, ranking, disclosure, terms và legal claims vẫn cần người duyệt.
- [ ] Business owner ký và đặt ngày rà quyền.

Các level này áp dụng cho **quyền tự thay đổi/vận hành nền**, không áp dụng cho request-response deterministic đã được duyệt như utility tính kết quả hoặc router xử lý một click thật. Day 84 không bắt buộc đạt L3. Giữ L1/L2 là quyết định đúng nếu chưa đủ thời gian, số lần chạy hoặc bằng chứng.

