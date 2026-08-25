# Affiliate Program Review

Tạo một bản review riêng cho mỗi tổ hợp **merchant/program + property/domain + channel**. Không dùng một phê duyệt chung cho các domain, app, email hoặc thị trường khác nhau.

## Identity, agreements và evidence

- Program ID:
- Record version:
- Merchant/vendor:
- Affiliate network/platform:
- Official program URL:
- Network/platform agreement URL:
- Network/platform agreement version/effective date:
- Merchant/vendor/channel-specific agreement URL:
- Merchant/vendor/channel-specific agreement version/effective date:
- `program_policy_urls_json` và version/effective date, hoặc child records:
- `agreement_order_of_precedence_json`, hoặc ordered child records:
- Terms valid from / valid until:
- Last terms checked at / checked by:
- Terms accepted at / accepted by account:
- Acceptance evidence reference:
- `source_evidence_ids_json`, hoặc child records:
- Change-notice channel và owner theo dõi:
- `open_change_request_ids_json`, hoặc child records:

> Không suy ra quyền từ trang giới thiệu hoa hồng. Phải đọc cả agreement của network và điều khoản riêng của merchant/channel; điều khoản cụ thể hơn hoặc mới hơn có thể kiểm soát.

## Eligibility và trạng thái tài khoản

- Publisher country/legal entity:
- Target/customer countries:
- Property/domain/app/channel submitted:
- Property approval status: `APPROVED / NOT_REQUIRED_WITH_CLAUSE / PENDING / REJECTED / EXPIRED`
- Property approval evidence/date/expiry:
- Required traffic/content:
- KYC/tax/payment prerequisites:
- Program account status: `DISCOVERED / APPLIED / APPROVED / REJECTED / SUSPENDED / CLOSED`
- Validity status: `CURRENT / EXPIRING / EXPIRED / UNKNOWN / CHANGED_REVIEW_REQUIRED`

## Allowed/prohibited channels và link behavior

Điền `YES / NO / CONDITIONAL / UNKNOWN`, kèm clause/URL chính thức và version. `UNKNOWN` nghĩa là `HOLD`, không phải được phép.

| Channel/action | Allowed? | Source clause/URL + version | Conditions/notes |
|---|---|---|---|
| Organic SEO/content |  |  |  |
| Utility/tool/app |  |  |  |
| AI-assisted content |  |  |  |
| PPC/direct linking |  |  |  |
| Brand keyword bidding |  |  |  |
| Email/SMS/push |  |  |  |
| Social/video |  |  |  |
| Coupon/deal/incentive |  |  |  |
| Self/client/employee referral |  |  |  |
| Deep link |  |  |  |
| Internal click router/redirect |  |  |  |
| URL shortening/cloaking |  |  |  |
| Sub-ID/clickref |  |  |  |
| UTM/query parameters |  |  |  |
| Automated link/price/status checks |  |  |  |

Trong Link Registry, `link_type` chỉ nhận `AFFILIATE / OFFICIAL_NON_AFFILIATE / FALLBACK`; `delivery_mode` chỉ nhận `DIRECT / ROUTER`. Không dùng các giá trị gộp như `AFFILIATE_ROUTER`. Review riêng việc program có cho phép router/redirect hay không.

## Economics và attribution

- Commission type: `ONE_TIME / RECURRING / HYBRID`
- Rate/amount và commission base:
- Cookie/attribution window:
- Attribution model và cross-device limits:
- Validation/locking period:
- Refund/reversal/clawback:
- Recurring duration/conditions:
- Payout schedule/threshold/currency:
- Payment methods khả dụng:
- Termination/suspension effect on pending/final balance:

## Tracking, privacy và records

- Official link format:
- Affiliate delivery modes allowed: `DIRECT / ROUTER / NONE / UNKNOWN`
- Sub-ID/clickref allowed fields, length và prohibited data:
- Cookie/pixel/browser-storage obligations:
- Consent/CMP obligations theo target market:
- API/webhook/postback/CSV fields và authentication:
- Data-controller/processor roles hoặc DPA reference:
- Cross-border transfer/retention/deletion obligations:
- Recordkeeping/audit/cooperation requirements:
- Policy/terms change-notice process:
- Network/merchant contact và escalation path:
- Sandbox/test order/test-link method:

> Không đưa email, tên, số điện thoại hoặc dữ liệu định danh trực tiếp vào sub-ID/UTM/clickref. Pseudonymous ID vẫn có thể là personal data và phải có purpose, quyền truy cập, retention/deletion phù hợp.

## Assets, claims và disclosure

- Logo/trademark/screenshot/copy rights:
- Price/API/feed cache/refresh requirements:
- Required disclosure wording/placement:
- Required brand/bid exclusions:
- Claims, coupon hoặc price restrictions:
- Backup official non-affiliate URL:

## Storage và operational control

- `affiliate_url_storage_type` cho Link Registry: `PUBLIC_VALUE / SECRET_REFERENCE / UNKNOWN`
- `affiliate_url_public_value` được phép lưu/render: `NO / YES / CONDITIONAL`
- `affiliate_url_secret_reference` requirement và storage owner:
- `affiliate_url_fingerprint` method/reference:
- Credential/token present: `NO / YES` (nếu `YES`, chỉ lưu secret reference; không chép value vào review):
- Agreement/acceptance evidence storage reference:
- Data/records retention period và deletion method:
- Kill switch owner/reference:

## Risk và decision

- Điểm chưa rõ:
- Policy risk `0–5`:
- Payout suitability `0–5`:
- Tracking quality `0–5`:
- Product/audience fit `0–5`:
- Decision: `GO / HOLD / NO-GO`
- Reason:
- `approved_properties_or_domains_json`, `approved_channels_json`, market arrays, hoặc child records:
- `allowed_link_types_json`: `AFFILIATE / OFFICIAL_NON_AFFILIATE / FALLBACK`
- `allowed_delivery_modes_json`: `DIRECT / ROUTER`
- Reviewer/date:
- Valid from / valid until:
- Next terms review date:
- `open_change_request_ids_json`, hoặc child records:

Chỉ `GO` khi account active; property/channel đã được duyệt nếu terms yêu cầu hoặc trạng thái `NOT_REQUIRED_WITH_CLAUSE` có evidence; agreement còn hiệu lực; các nghĩa vụ privacy/tracking đã được xác định; và không còn thay đổi vật chất đang chờ review. Nếu chưa rõ quyền, terms hết hạn/đổi, hoặc acceptance/approval/exemption không chứng minh được: `HOLD`. Nếu bị cấm rõ ràng: `NO-GO`.

