# Thư viện biểu mẫu

## Cách dùng

1. Không điền trực tiếp lên file mẫu gốc. Sao chép sang thư mục làm việc và thêm ngày/ID.
2. Các file CSV có thể mở bằng Excel/Google Sheets. Luôn import/export UTF-8 để giữ tiếng Việt; đặt cột ID và timestamp ở kiểu **Text** để Excel không tự đổi giá trị.
3. Không lưu password, API key, KYC, số tài khoản, ảnh giấy tờ hoặc direct/sensitive identifiers. Các record event/recommendation/conversion có thể chứa pseudonymous user/transaction data: chỉ lưu ở nơi private có kiểm soát truy cập, khai báo trong Data/Privacy Inventory, đặt retention/deletion và không commit/public.
4. URL evidence được ưu tiên hơn việc chép nguyên nội dung nguồn; chỉ lưu snapshot khi quyền/điều khoản cho phép.
5. Mọi quyết định `GO/SCALE/PUBLISH` cần tên người duyệt và ngày.
6. Đây là schema trắng. Trước khi nhập dữ liệu thật, đọc [ví dụ xuyên suốt đã điền](../12-vi-du-xuyen-suot.md).

| File | Dùng cho | Tạo lần đầu |
|---|---|---|
| [01-project-charter.md](01-project-charter.md) | Khóa mục tiêu, phạm vi, ngân sách, quyền hạn | Day 1 |
| [02-research-log.csv](02-research-log.csv) | Lưu bằng chứng nhu cầu | Day 10 |
| [03-opportunity-card.md](03-opportunity-card.md) | Chấm và chọn problem/utility | Day 13–21 |
| [04-affiliate-program-review.md](04-affiliate-program-review.md) | Đọc terms và quyết định program | Day 24 |
| [04b-program-registry.csv](04b-program-registry.csv) | Registry version/validity/acceptance/phê duyệt của từng program | Day 24 |
| [05-golden-tests.csv](05-golden-tests.csv) | Expected results cho scoring | Day 34 |
| [06-source-registry.csv](06-source-registry.csv) | Danh sách nguồn đã duyệt | Day 29 |
| [07a-products.csv](07a-products.csv) | Danh mục sản phẩm | Day 30 |
| [07b-plans.csv](07b-plans.csv) | Giá/gói theo vùng và kỳ thanh toán | Day 30 |
| [07c-feature-evidence.csv](07c-feature-evidence.csv) | Feature/limitation gắn evidence | Day 30 |
| [07d-source-evidence.csv](07d-source-evidence.csv) | Evidence theo field/claim, version, validity và storage | Day 30 |
| [08-link-registry.csv](08-link-registry.csv) | Quản lý `link_id`, `link_type`, `delivery_mode` và URL storage | Day 52 |
| [09-publishing-approval.md](09-publishing-approval.md) | Sign-off trước publish | Day 58 trở đi |
| [10-experiment-card.md](10-experiment-card.md) | Định nghĩa thử nghiệm trước khi chạy | Day 74 |
| [11-weekly-review.md](11-weekly-review.md) | Review tuần theo các tầng | Mỗi 7 ngày |
| [12-daily-run-log.md](12-daily-run-log.md) | Log một job/bot run | Day 78 trở đi |
| [13-incident-report.md](13-incident-report.md) | Sự cố và postmortem | Khi có sự cố |
| [14a-orders.csv](14a-orders.csv) | Order/order line chuẩn hóa | Khi program hoạt động |
| [14b-commission-transitions.csv](14b-commission-transitions.csv) | Lịch sử commission append-only, gồm reversal | Khi import network |
| [14c-payouts.csv](14c-payouts.csv) | Payout/statement và gross/net/fee | Khi có payout |
| [14d-payout-allocations.csv](14d-payout-allocations.csv) | Phân bổ nhiều commission ↔ nhiều payout | Khi đối soát payout |
| [15a-activity-snapshot.csv](15a-activity-snapshot.csv) | Traffic, utility, job và data quality theo lịch | Day 52 trở đi |
| [15b-cohort-funnel.csv](15b-cohort-funnel.csv) | Matured click cohort → order → valid → final | Khi affiliate active |
| [15c-payout-snapshot.csv](15c-payout-snapshot.csv) | Final due → gross allocated → net cash received | Khi có payout |
| [16-decision-log.csv](16-decision-log.csv) | Lưu quyết định lớn | Từ Day 1 |
| [17-content-brief.md](17-content-brief.md) | Brief trang people-first có nguồn | Day 57 trở đi |
| [18-data-privacy-inventory.csv](18-data-privacy-inventory.csv) | Inventory event/cookie/processor/retention | Day 54 |
| [19-cost-log.csv](19-cost-log.csv) | Chi phí và giờ vận hành | Từ Day 1 |
| [20-content-inventory.csv](20-content-inventory.csv) | Trang, evidence, link và freshness | Day 57 |
| [21-event-contract.csv](21-event-contract.csv) | Trigger/params/privacy của events | Day 50 |
| [22-recommendation-run.csv](22-recommendation-run.csv) | Audit một lần chạy recommendation | Day 45 trở đi |
| [23-change-request.md](23-change-request.md) | Diff, impact, approval và rollback | Khi data/rule/page đổi |
| [24-scoring-rules.csv](24-scoring-rules.csv) | Hard/soft rule, rubric, reason template và version | Day 32 |
| [25-dataset-release-manifest.csv](25-dataset-release-manifest.csv) | Khóa exact record/version/hash của một release | Day 35 |
| [26-recommendation-component-scores.csv](26-recommendation-component-scores.csv) | Component scores của từng candidate/run | Day 43 trở đi |
| [27-event-click-log.csv](27-event-click-log.csv) | Event/click append-only nối journey, run và link | Day 50 trở đi |
| [28-network-status-mapping.csv](28-network-status-mapping.csv) | Map status gốc của network sang trạng thái chuẩn | Trước import thật |
| [29-stack-rules.md](29-stack-rules.md) | Coverage, compatibility, overlap, budget và tie rules | Day 32 |

## Quy ước cho các registry compliance

- Timestamp dùng ISO 8601 có timezone, ưu tiên UTC, ví dụ `2026-08-25T08:30:00Z`. `valid_from`, `valid_until` và `next_check_at` không thay nhau.
- `record_version` tăng khi nội dung/decision thay đổi; riêng Link Registry dùng `link_version`. Không sửa đè record/version đã dùng để publish mà không giữ audit trail.
- `validity_status`: `CURRENT / EXPIRING / EXPIRED / UNKNOWN / CHANGED_REVIEW_REQUIRED`. `UNKNOWN`, hết hạn hoặc có material `open_change_request_ids_json` nghĩa là `HOLD`.
- `storage_class`: `PUBLIC / INTERNAL / CONFIDENTIAL / SECRET`. Affiliate URL không mặc nhiên là secret; token, password và credential luôn là `SECRET` và registry chỉ lưu secret reference.
- `link_type`: `AFFILIATE / OFFICIAL_NON_AFFILIATE / FALLBACK`; `delivery_mode`: `DIRECT / ROUTER`. Hai chiều này độc lập: không mã hóa delivery mode vào link type. Router chỉ dùng khi program cho phép và final domain nằm trong allowlist.
- Với `link_type=AFFILIATE`, `affiliate_url_storage_type` là `PUBLIC_VALUE` hoặc `SECRET_REFERENCE`, và chỉ điền đúng một trong `affiliate_url_public_value`/`affiliate_url_secret_reference`. Với link không affiliate, dùng `NONE` và để trống cả hai. `affiliate_url_fingerprint` chỉ phục vụ đối chiếu/version; fingerprint không thay access control và không được coi là cách che một secret an toàn.
- `public_href` là URL/path gắn vào CTA: direct dùng URL được phép công khai; router dùng `/go/{link_id}`. `destination_url` là landing page dự kiến để kiểm health/redirect, không phải nơi lén lưu lại tracking URL nhạy cảm.
- `robots_decision`: `ALLOW / DISALLOW / HOLD / NOT_APPLICABLE`; lỗi `5xx`/unreachable dùng `HOLD` theo fail-closed, còn `401/403/429` dừng và human review.
- `identifier_type`: `NONE / DIRECT / PSEUDONYMOUS / ANONYMOUS`. Chỉ chọn `ANONYMOUS` khi có `anonymization_evidence_reference`; tên ID ngẫu nhiên không đủ.
- Field có hậu tố `_json` phải là JSON array, ví dụ giá trị logic `["EEA","UK"]`; trong CSV phải quote và escape thành `"[""EEA"",""UK""]"`. Không dùng dấu `|`, danh sách comma tự do hoặc chuỗi prose thay cho array. Khi danh sách lớn/cần thuộc tính riêng, tạo child table một record/một giá trị và lưu reference thay vì nhét vào cell.

## ID convention

```text
OPP-YYYYMMDD-001  opportunity
SRC-YYYYMMDD-001  source
PRD-001           product
PLN-001           plan
EVD-YYYYMMDD-001  evidence
PGM-001           program
OFF-001           offer nếu program/network có khái niệm này
LNK-001           public/affiliate link
JNY-...           utility journey
REC-...           recommendation run
EVT-...           event
CLK-...           outbound click
JOB-...           scheduled/manual bot job
IMP-...           import batch
ORD-...           normalized order/order line
COM-...           commission
PAY-...           payout
CHG-...           change request
REL-...           dataset/release manifest
EXP-...           experiment
INC-...           incident
DEC-...           decision
```

ID phải duy nhất, ổn định và không chứa email/tên/số điện thoại của người dùng. Dùng `pseudonymous_session_id`; không gọi ID là anonymous nếu chưa có bằng chứng anonymization. Đặt `storage_class`, xác định `identifier_type`, và nếu còn khả năng nối lại thì quản lý như pseudonymous/personal data theo review áp dụng.

