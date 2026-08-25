# 09 — Kiến trúc MVP và tự động hóa có kiểm soát

## 1. Kiến trúc mặc định cho 90 ngày

MVP không cần tám “AI agent”, backend, database, login hay router. `Beginner Track` dùng một pipeline nhỏ, deterministic và audit được:

```text
Nguồn chính thức đã duyệt
→ CSV quản trị có schema/version/evidence
→ validate + human approval
→ build public JSON + Dataset Release Manifest
→ static website/form
→ hard filters + scoring rules + stack rules deterministic
→ result + reason codes + data/version date
→ direct approved product/affiliate link
→ on-site event + affiliate-network reconciliation
```

Router `/go/{link_id}` là nhánh tùy chọn, không phải điều kiện launch. Chỉ thêm khi program cho phép, có nhu cầu server-side mapping thật và một người đủ năng lực duyệt security. AI chỉ hỗ trợ tìm ứng viên, tóm tắt, tạo draft hoặc phát hiện bất thường; dữ liệu, rule, thứ tự kết quả, link và quyền publish phải đi qua artifact/version đã duyệt.

Phạm vi thực tế Day 90 là một utility public, 12–15 sản phẩm (`LEAN`: 8), 5 decision assets (`LEAN`: 3), tracking đủ đối soát và các job L1/L2 an toàn. Không hứa đạt traffic, commission hoặc L3.

## 2. Control plane và phân vai

```text
                           HUMAN CHANGE CONTROL
       ICP/Terms/KYC/Tax/Budget/Data/Rule/Link/Publish/Incident/Scale
                                      │ approve exact versions
                                      ▼
Approved CSV/rules ──validate/build──> Public JSON + manifest
                                      │
                                      ▼
Visitor ──> Static form ──> Deterministic engine ──> Result/Compare
                                                        │
                                                        ▼
                                              Direct approved link
                                              (router is OPTIONAL)

Search Console ─┐
Web events ─────┼──> KPI/Reconciliation ──> Weekly Review ──> Decision Log
Network export ─┤
Payout proof ───┘
```

| Vai trò | Được làm | Không tự quyết |
|---|---|---|
| Bạn/chủ dự án | Chọn ICP/scope, kiểm nguồn, duyệt data/rules/link/copy/terms/budget, tạo account, publish/rollback | Không giao trách nhiệm pháp lý/tài chính cho bot |
| Coding agent | Build đúng [Work Packet Day 43–56](03-lo-trinh-90-ngay.md#work-packet-day-4356-cho-coding-agent), tạo preview/test/manifest/rollback evidence | Không tự sửa business rule, nguồn, giá, destination, disclosure hoặc cấp quyền production |
| Research/content agent | Tìm candidate, chuẩn hóa draft và gắn nguồn vào staging | Không biến draft thành fact hoặc tự publish material claim |
| Runtime | Tính cùng input + exact versions thành cùng output; render/event/open allowlisted link | Không tự thay data/rule/link/config production |

### Runtime authority khác change authority

- **Runtime authority** cho phép code đã publish phục vụ từng request mà không xin duyệt từng lượt.
- **Change authority** là quyền thay data, scoring, stack constraints, link, copy hoặc config production. Quyền này được cấp theo action/risk/allowlist và luôn có expiry/review/rollback.
- Một engine chạy đúng 10.000 lượt vẫn không tự có quyền sửa price/ranking. Một job L3-limited cũng không được thừa hưởng quyền của runtime sang action khác.

## 3. Thành phần MVP

| Thành phần | Trách nhiệm | Không được làm |
|---|---|---|
| Static Website/UI | Thu input, validate, hiển thị result/trade-off/disclosure/date/version | Không bịa result khi file/rule lỗi |
| Deterministic Build | Validate CSV, loại secret, xuất exact public JSON + manifest | Không “sửa hộ” source data trong lúc build |
| Recommendation Engine | Hard filter, component score, valid-stack build và canonical outputs | Không dùng commission để rank/tie-break |
| Versioned Dataset | Product/plan/feature/source/status/provenance | Không có critical value thiếu evidence |
| Scoring Rules | Anchor 0–5, weight, UNKNOWN behavior, reason code | Không để logic ngầm chỉ nằm trong code/prompt |
| Stack Rules | Coverage, duplicate, compatibility, cost, floor, tie/output rules | Không hạ floor để luôn có kết quả |
| Explanation Layer | Render deterministic reasons/limitations | Không cho LLM thêm experience claim/fact |
| Link Registry | Tách link type khỏi delivery mode; giữ approved destination/domain, status, version, fallback | Không nhận arbitrary user destination |
| On-site Events | Ghi hành vi site với run/version/link type | Không là source of truth cho order/payment |
| Network Ledger | Order/valid/final/paid history | Không suy diễn pending thành revenue |
| Scheduler/Workers | Freshness/link/report checks, tạo diff/queue | Không auto-publish material changes |
| Approval Queue | Before/after/evidence/impact/reviewer | Không tự phê duyệt thay con người |
| Logs/Alerts/Kill Switch | Quan sát, dừng và phục hồi | Không ghi secret hoặc PII |
| Affiliate Router | **Tùy chọn:** allowlisted redirect + mapping | Không open redirect, auto-click hoặc cookie stuffing |

## 4. Các nguồn sự thật và contract version

| Câu hỏi | Source of truth | Không dùng thay thế |
|---|---|---|
| Sản phẩm nào active? | [Products](templates/07a-products.csv) | Copy trên trang/content |
| Giá/gói/actual charge? | [Plans](templates/07b-plans.csv) | Monthly equivalent đứng một mình |
| Feature/limitation có thật? | [Feature Evidence](templates/07c-feature-evidence.csv) + [Source Evidence](templates/07d-source-evidence.csv) | LLM summary không evidence |
| Nguồn có được dùng/còn mới? | [Source Registry](templates/06-source-registry.csv) | URL rời không checked date |
| Program/channel/status hợp lệ? | Program Review + [Program Registry](templates/04b-program-registry.csv) | Có affiliate URL không đồng nghĩa approved |
| CTA dẫn đi đâu? | [Link Registry](templates/08-link-registry.csv) | URL hard-code rải rác trong page |
| Điểm/reason tính thế nào? | [Scoring Rules](templates/24-scoring-rules.csv) | Logic ngầm trong UI/prompt |
| Stack nào hợp lệ/canonical output? | [Stack Rules](templates/29-stack-rules.md) | “Top products” ghép tùy ý |
| Release gồm file/version nào? | [Dataset Release Manifest](templates/25-dataset-release-manifest.csv) | Tên file “final-v2-new” |
| Điểm cụ thể đến từ đâu? | [Recommendation Component Scores](templates/26-recommendation-component-scores.csv) + [Recommendation Run](templates/22-recommendation-run.csv) | Chỉ lưu total score |

Mỗi recommendation run phải gắn tối thiểu: `input_schema_version`, `data_version`, `scoring_version`, `stack_rules_version`, `link_registry_version` và `page/code release version`. Nếu artifact hiện chưa có cột tương ứng, thêm vào schema/version mới trước launch; không nhét nhiều loại version vào một trường mơ hồ như `model_version`.

Dataset Manifest khóa exact filenames, counts, schema/data/scoring/stack/link versions, checksum/hash, freshness coverage, Golden Test result, reviewer, approved time và rollback version. Build fail nếu thiếu hoặc tham chiếu version không khớp. Preview trước Day 52 có thể dùng một Link Registry fixture được version và ghi rõ `TEST_FIXTURE`; fixture không được chứa/đi tới production affiliate destination, và không thể qua Gate 7.

`release_status` dùng `DRAFT / APPROVED / RETIRED / ROLLED_BACK`; chỉ `APPROVED` được public. `UNKNOWN` là trạng thái dữ liệu/tri thức ở field theo schema, không phải cách lách approval của release.

## 5. Data model tối thiểu

### `product`

```text
product_id, vendor_id, product_name, category, official_url
status, regions, languages, platforms, target_personas, use_cases
critical_limitations, official_source_id
last_verified_at, next_check_at, evidence_confidence
record_version, valid_from, valid_to, reviewer
```

`status` là lifecycle/QA status được định nghĩa trong Data Dictionary, ví dụ `DRAFT/ACTIVE/BLOCKED/RETIRED`; freshness tính từ evidence/checked/next-check policy, không suy từ tên status. Không dùng `ELIGIBLE` như record status: eligibility là kết quả được tính cho từng input + versions.

### `plan`

```text
plan_id, product_id, plan_name, region, currency, billing_interval
pricing_model, price_unit, actual_charge_amount, annual_equivalent_monthly_amount
per_seat_amount, included_seats, minimum_seats, usage_tiers
tax_status, trial/refund/quota
source_id/source_url, checked_at/effective_at/next_check_at
confidence, status, record_version, reviewer
```

Nếu `team_size > 1`, rule actual charge phải xử lý rõ `FLAT`, `PER_SEAT`, minimum/included seats và usage tier. Nếu chưa có evidence/formula đáng tin cậy, MVP khóa `team_size = 1`; không nhân đơn giản một giá “starting at”. Hiển thị `actual charge/upfront` và `monthly equivalent` riêng.

### `feature_evidence`

```text
evidence_id, product_id/plan_id, field_or_feature_key
normalized_value, value_type, is_must_have_relevant, is_limitation
source_id/source_url/source_tier, evidence_summary
checked_at/effective_at/next_check_at, confidence, status
conflict_reference, record_version, checked_by
```

### `scoring_rule`

```text
rule_id, rule_version, rule_type, component
input predicate, product/evidence predicate
hard_filter_result hoặc score_0_to_5 + weight
reason_code, deterministic reason_template
evidence_keys_required_json, unknown_behavior, priority, status
```

### `stack_rule`

```text
stack_rules_version
need groups + must-have coverage
duplicate/overlap/compatibility constraints
actual/upfront/monthly-equivalent budget constraints
Valid Stack Floor + minimum fit
canonical output selection + tie order
```

### `link`

```text
offer_id, offer_version, product_id, program_id
link_type = AFFILIATE | OFFICIAL_NON_AFFILIATE | FALLBACK
delivery_mode = DIRECT | ROUTER
approved channel/domain, terms/evidence version
approved public URL hoặc secret reference theo storage policy
destination URL/final domain, effective/expiry/status
sub-ID rule, disclosure version, rel, fallback
approved_by/approved_at/next_terms_review_at
```

### `recommendation_run`

```text
recommendation_run_id, occurred_at, normalized_inputs/input hash
input_schema/data/scoring/stack/link/page versions
candidate IDs + exact rejection reasons
component score references + evidence IDs
Best Fit/Cheapest Qualified/Alternative/(optional Maximum Automation)
actual charge/monthly equivalent/assumptions/limitations
confidence components + output snapshot hash
is_internal_or_test, retention policy
```

### `click/order/commission/payout`

Dùng append-only history theo [KPI/ledger spec](07-kpi-va-unit-economics.md). Event/site log chỉ chứng minh hành động trên site; affiliate network là nguồn sự thật của order/commission; payout proof là nguồn sự thật của tiền đã nhận. Không lưu PII vào event/sub-ID.

## 6. Freshness, provenance và release validator

Phân loại field trước khi build:

- **Critical:** price/actual charge/billing/upfront, region, must-have, compatibility/hard constraint, destination/status, program/disclosure rule và field có thể đổi eligibility/cost/ranking.
- **Noncritical:** nội dung hỗ trợ không làm đổi eligibility/cost/ranking/disclosure/destination.

Release chỉ pass khi:

- Critical evidence/freshness coverage = **100%**; critical `UNKNOWN/STALE/CONFLICT/BLOCKED` làm record/stack không public.
- Noncritical freshness coverage ≥**98%**; phần còn lại bị ẩn hoặc gắn trạng thái, không trình bày như dữ liệu mới.
- Referential integrity của product/plan/evidence/source/rule/link IDs pass.
- Enum/type/range/date/version schema pass; duplicate stable IDs và orphan reference = 0.
- Golden Tests pass theo gate; manifest/checksum/rollback version tồn tại.
- Public JSON không chứa credential, secret reference không được public, PII hoặc raw evidence bị cấm phân phối.

Các tỷ lệ là starter thresholds an toàn của dự án, không phải benchmark độ chính xác toàn ngành. Thay đổi chỉ áp dụng prospective qua Decision Log + schema/rule version mới.

## 7. File tĩnh hay database?

### Giữ versioned CSV/JSON trong Day 90 khi

- 8–15 sản phẩm, một owner và cập nhật thủ công/bán tự động.
- Không cần lưu account/PII của user.
- Diff/review/rollback quan trọng hơn ghi đồng thời.
- Static UI có thể tính rule trong browser hoặc dùng JSON đã build.

Đây là default. CSV quản trị có thể private; bước build chỉ xuất field được phép public. Hosting cần HTTPS, preview, version history và rollback, nhưng không cần server chạy liên tục.

### Chỉ chuyển managed database/backend khi

- Nhiều người/job phải ghi đồng thời hoặc referential integrity không còn quản lý được bằng build validator.
- Cần auth/admin, server-side ledger, permission audit hoặc scheduler phức tạp.
- Router/server-side conversion mapping có use case và Gate 7 riêng.
- Đã có export/backup/migration/restore plan, budget và người vận hành.

Không chuyển chỉ vì “database chuyên nghiệp hơn”.

## 8. Beginner Track executable

Chủ dự án không cần chọn framework. Chỉ cần giao input/output/acceptance:

1. Điền và duyệt CSV/rules/templates bằng công cụ bảng tính/text quen thuộc.
2. Giao exact files + versions cho coding agent theo Work Packet.
3. Nhận preview URL, test report, release manifest và rollback instruction.
4. Kiểm các Golden scenarios trên giao diện; đối chiếu score/reason/source/link bằng mẫu.
5. Duyệt production release; sau deploy chạy smoke test và lưu evidence.

Scorecard chọn giải pháp Day 6/43:

| Tiêu chí | Trọng số |
|---|---:|
| Golden/valid-stack tests deterministic | 20% |
| Dễ kiểm/vận hành cho owner | 15% |
| Version/preview/rollback | 15% |
| CSV/JSON export/backup/no lock-in | 10% |
| Secret/access/log/security | 15% |
| Custom event/consent | 10% |
| Recurring cost trong budget | 10% |
| Tài liệu/support | 5% |

Hard blockers: không export/rollback/test được, source data bị khóa trong prompt đen, secret ở public file, cost vượt budget, hoặc terms không cho use case. No-code/low-code vẫn phải qua cùng acceptance; nếu không biểu diễn được hard filters/version/audit thì không dùng cho production.

## 9. Recommendation algorithm v0

### Bước 1 — Validate và normalize input

```text
market ∈ supported markets
currency ∈ supported currencies
monthly_budget ≥ 0
max_upfront_payment ≥ 0
billing_preference ∈ monthly_only | annual_allowed
must_have_needs không rỗng
team_size = 1, trừ khi team pricing rules đã được duyệt
```

Lưu normalized input/hash và `input_schema_version`. Không gửi free text/PII vào analytics.

### Bước 2 — Hard filters

Loại plan/product nếu inactive/retired; market không phục vụ; thiếu must-have; actual/upfront cost vượt hard limit; billing không phù hợp; currency không so được bằng snapshot/rule đã duyệt; critical evidence fail freshness/conflict/status; hoặc incompatible với hard constraint/stack hiện có.

Hard filter chạy trước mọi score. `UNKNOWN` dùng behavior đã duyệt (`BLOCK/NO_SCORE/allowed fallback`), không tự đổi thành 0/No.

### Bước 3 — Chấm component và User Fit

Trong Scoring Rules, dùng enum đã ghi trong Data Dictionary, tối thiểu: `rule_type = HARD_FILTER | COMPONENT | REASON_ONLY` và `score_scope = USER_FIT | EVIDENCE_CONFIDENCE | AUTOMATION | COMMERCIAL_INTERNAL`. `COMMERCIAL_INTERNAL` luôn có `public_ranking_allowed=false`; hard filter không bị điểm component ghi đè. Mọi row cùng một release phải trỏ đúng `scoring_version`.

```text
UserFit = 20 × (
  0.30 × NeedCoverage_0_to_5
+ 0.20 × BudgetFit_0_to_5
+ 0.15 × WorkflowTeamFit_0_to_5
+ 0.15 × ValueForCost_0_to_5
+ 0.10 × EaseOfAdoption_0_to_5
+ 0.05 × IntegrationFit_0_to_5
+ 0.05 × RiskLimitationsFit_0_to_5
)
```

Mỗi component phải dùng anchor cụ thể trong Scoring Rules:

| Điểm | Anchor chung để viết rule chi tiết |
|---:|---|
| 0 | Evidence xác nhận không đáp ứng; hard constraint vẫn phải loại trước |
| 1 | Chỉ đáp ứng phần rất nhỏ, gap lớn |
| 2 | Đáp ứng một phần, trade-off đáng kể |
| 3 | Đạt baseline đã mô tả cho use case |
| 4 | Đáp ứng mạnh, ít trade-off quan trọng |
| 5 | Đạt mức cao nhất đã định nghĩa, có evidence trực tiếp |

Khung chung không thay cho rubric component. Mỗi điểm phải lưu rule ID, evidence IDs, weight, weighted points và reason code trong Component Scores. Commercial Score/commission chỉ dùng báo cáo nội bộ; không đi vào User/Stack Fit, highlight hoặc tie-break.

**Fit Score là tương đối** trong candidate set + normalized input + exact dataset/scoring/stack-rules versions của lần chạy. Nó không phải xác suất mua thành công, điểm chất lượng tuyệt đối, benchmark thị trường hay bảo đảm.

### Bước 4 — Tính Evidence Confidence riêng

```text
EvidenceConfidence =
  0.40 × Completeness
+ 0.30 × Freshness
+ 0.20 × SourceAuthority
+ 0.10 × Consistency
```

Mỗi thành phần dùng cùng scale 0–5 hoặc 0–100 trong một version; không trộn scale. Rubric khởi điểm:

- `Completeness`: tỷ lệ critical fields có evidence đúng scope.
- `Freshness`: còn trong SLA đã duyệt, dựa vào checked/next-check time.
- `SourceAuthority`: official pricing/terms/docs trực tiếp cao nhất; official support/changelog tiếp theo; nguồn thứ cấp chỉ corroboration; community chỉ là tín hiệu.
- `Consistency`: nguồn/evidence cùng scope đồng thuận, không conflict chưa giải quyết.

Critical blocker luôn thắng điểm tổng. Stack Confidence dùng mức thấp nhất của critical claims quyết định eligibility/cost/ranking; không lấy trung bình để che một claim yếu.

### Bước 5 — Tạo valid stacks

Áp dụng đúng [Stack Rules](templates/29-stack-rules.md):

1. Chỉ dùng candidate đã qua hard filters.
2. Cover 100% must-have need groups.
3. Không ghép duplicate/overlap bị cấm; compatibility rules pass.
4. Mọi product/plan active, critical freshness 100%, không critical conflict/unknown.
5. Tính actual charge, upfront và monthly equivalent không đếm trùng; tất cả hard budgets pass.
6. Số công cụ và minimum Stack Fit qua `Valid Stack Floor` đã duyệt trước.

### Bước 6 — Chọn output canonical

- `Best Fit`: valid stack có Stack Fit cao nhất.
- `Cheapest Qualified`: valid stack có tổng monthly equivalent thấp nhất nhưng vẫn qua cùng Valid Stack Floor và minimum fit; actual/upfront charge vẫn hiển thị riêng và phải qua hard limit. Không hạ sàn để tạo lựa chọn rẻ.
- `Alternative`: valid stack khác Best Fit, có trade-off có ý nghĩa được reason code mô tả.
- `Maximum Automation`: chỉ hiện khi có automation rubric 0–5 + evidence + Golden Tests đã duyệt cho toàn candidate liên quan; thiếu bất kỳ phần nào thì **bỏ output**, không suy đoán. Đây là nhãn recommendation, không phải cấp L3/change authority cho hệ thống.

Tie order không thương mại: Evidence Confidence cao hơn → tổng monthly equivalent thấp hơn → actual/upfront charge thấp hơn → ít tools hơn → stable stack ID. Có thể hiển thị đồng hạng khi chênh lệch không có ý nghĩa.

### Bước 7 — Explain deterministic trước, LLM sau

Engine tạo reason/limitation từ `reason_code + reason_template + input + approved field/evidence`. LLM, nếu dùng, chỉ được diễn đạt lại sau khi stack/order/facts đã khóa; không được thêm/bớt claim, thay ranking hoặc giấu limitation. Lưu prompt/model/template version, deterministic input reasons và LLM output để audit; khi LLM fail thì hiển thị deterministic text.

### Pseudocode dễ kiểm

```text
input = validate_and_normalize(user_answers, input_schema_version)
release = load_exact_manifest_versions()
candidates = hard_filter(active_products(release), input)

for candidate in candidates:
    components = score_by_versioned_rules(candidate, input, evidence)
    audit(component_scores, rule_ids, evidence_ids)

valid_stacks = build_and_validate_stacks(candidates, stack_rules_version)
results = select_best_cheapest_alternative(valid_stacks)
if automation_rubric_and_evidence_and_tests_pass:
    results += select_maximum_automation(valid_stacks)

reasons = render_deterministic_reason_codes(results)
return result_with_versions_manifest_confidence(reasons)
```

Không cần machine learning trước khi có dữ liệu đủ lớn, sạch, mature và một governance plan chứng minh nó không làm thiên lệch user fit.

## 10. Link contract: direct mặc định, router tùy chọn

### Common controls cho mọi link

- CTA lookup bằng stable `offer_id + offer_version` trong Link Registry; không hard-code URL rải rác.
- Chỉ link `ACTIVE`, trong effective/expiry time và đúng approved channel/domain.
- Disclosure ở gần recommendation/CTA; affiliate link có qualifying `rel` theo policy đã duyệt.
- Inactive/invalid dùng official non-affiliate fallback hoặc safe information page; không domain tùy ý.
- Không auto-open/popunder/forced click/cookie stuffing/self-test/self-purchase khi chưa được phép.
- Event `product_outbound_click` chứa `recommendation_run_id`, product/offer/version, `link_type`, `delivery_mode`, placement, destination domain và internal/test flag; không PII.
- Chỉ derive `affiliate_click` khi offer version active và `link_type=AFFILIATE`, bất kể `delivery_mode`. Site event không chứng minh network click/order.

### Direct approved link — mặc định

Flow:

```text
User click
→ lookup exact approved link record
→ emit/dedupe product_outbound_click
→ browser mở approved destination với delivery_mode=DIRECT
```

Ưu điểm: không backend/router/secret infrastructure, ít điểm lỗi và dễ kiểm cho người mới. Nếu program yêu cầu affiliate URL có token bí mật không được public, không dùng direct affiliate URL đó; dùng official non-affiliate link hoặc chỉ cân nhắc router sau security/terms review.

### Router `/go/{link_id}` — chỉ khi cần

Không hỗ trợ `/go?url=https://arbitrary.example`. Server phải:

1. Lookup stable offer trong allowlist; kiểm status/effective/expiry/channel/final domain.
2. Tạo idempotent outbound click ID và minimal log; timeout ngắn.
3. Gắn sub-ID đúng exact program rule, nếu được phép; không PII.
4. Redirect `302/307` theo design; offer fail dùng approved safe fallback.
5. Rate limit, secret isolation, monitoring, kill switch và rollback.

Gate 7 phải ghi branch đang dùng. Router fail không bắt utility fail nếu direct official/non-affiliate fallback đã duyệt; nhưng offer không được monetized cho đến khi branch hợp lệ.

## 11. Workflow cập nhật dữ liệu

```text
SCHEDULE/MANUAL TRIGGER
→ check Source Registry ACTIVE + terms/rate limits
→ fetch/manual capture vào staging
→ normalize schema + provenance
→ freshness/conflict/referential checks
→ diff + downstream impact
→ human review cho material change
→ Golden/impact tests
→ create manifest + preview
→ human approve exact release
→ publish + live verify
→ log/alert/rollback khi guardrail fail
```

Trong Day 90, price/plan, must-have feature, region, compatibility, scoring/ranking, destination/program terms và disclosure luôn là material. Chúng không auto-publish vì parser hoặc LLM “confident”.

## 12. Mức tự động hóa và portfolio bot

| Mức | Ý nghĩa | Ví dụ Day 90 |
|---|---|---|
| L0 | Làm tay hoàn toàn | Terms/KYC/tax/payout decision |
| L1 | Bot draft, người kiểm/apply | Research/content/data candidate |
| L2 | Bot chạy/check/report, material change chờ duyệt | Link/freshness monitor, KPI report, change queue |
| L3-LIMITED | Bot tự apply một action low-risk/reversible đã allowlist | Chỉ sau Gate 8 evidence; không phải mục tiêu mặc định |

| Vai trò “bot” | MVP | Authority mặc định |
|---|---|---|
| Opportunity Scout | Research Log candidates | L1 |
| Product Researcher | Approved-source draft vào staging | L1 |
| Data QA | Schema/freshness/conflict/diff | L2 flag/report |
| Product Judge | Deterministic rules + Golden Tests | Runtime tự tính; rule change human |
| Utility Engine | Static form/result/compare | Runtime tự phục vụ; production change human |
| Content Assistant | Evidence-based brief/draft | L1; publish human |
| Direct Link Renderer | Lookup allowlisted record và open destination | Runtime; link record change human |
| Optional Router | Allowlisted redirect/log | Runtime sau Gate 7; offer/config change human |
| Analytics Reporter | Events/import/dashboard/report | L2 |
| Experiment Assistant | Card/assignment/report | L1/L2; decision human |
| Portfolio Supervisor | Tóm tắt gate evidence | L1; budget/scale human |

`GO-L3-LIMITED` cần đúng một task reversible/allowlisted, ≥20 distinct supervised runs trên ≥14 ngày và ít nhất một full freshness/SLA cycle (nếu SLA dài hơn thì chờ lâu hơn), đủ case no-change/valid-change/duplicate/timeout-retry/auth-rate-limit-or-equivalent/conflict/rollback/kill, 0 critical error và owner ký scope/expiry. Mặc định Day 84 là `KEEP-L2`; xem [Gate 8](08-go-no-go.md#10-gate-8--safe-automation-day-84).

## 13. Quyền thay đổi theo risk

| Action | Risk | Day-90 change authority |
|---|---|---|
| Đánh dấu stale/khóa unsafe offer theo hard rule | Protective, reversible | L2 tự flag/pause + alert; human reopen |
| Tạo report/ticket/draft | Low | L1/L2 |
| Sửa typo không đổi nghĩa | Low | Human; L3-limited chỉ sau Gate 8 riêng |
| Thay price/feature/region/compatibility | Material | Human approve + impact tests |
| Thay score/weight/stack floor/ranking | Material | Human approve + full Golden Tests + new version |
| Thay destination/program/disclosure | Compliance/material | Human approve + link/live QA |
| Publish page/claim mới | High | Human approve |
| Accept terms/KYC/tax/payout | Legal/financial | Human only |
| Chi tiền/tăng subscription/ads | Financial | Human only |
| Mở market/ngách mới | Strategic/compliance | Human gate |

## 14. Security và privacy baseline

- MFA cho domain, hosting, repository, analytics, network và payment.
- Account riêng/quyền tối thiểu; revoke người/service không còn cần.
- Secret trong secret manager/environment phù hợp; không repo, public JSON, prompt, screenshot, event hoặc log.
- Validate input/output encode; coi user/fetched data là untrusted.
- Direct link dùng exact allowlist; router tùy chọn phải no-arbitrary-redirect + rate limit.
- HTTPS; staging noindex/access-controlled; production không lộ source/admin artifacts.
- Backup có version và restore test; release có rollback target.
- Audit log data/rule/link/publish/permission changes.
- Dependency/plugin update qua preview/test/rollback, không auto-update production mù.
- Kill switch riêng cho source job, publish, link/offer/router, analytics tag và automation worker.

## 15. Release pipeline và Definition of Done

```text
Approved change request
→ staging CSV/rules/link updates
→ schema/reference/freshness/secret validation
→ deterministic public build + manifest
→ preview
→ Golden/stack/link/event/mobile/compliance tests
→ human approval exact versions
→ backup + release tag
→ production deploy
→ live smoke test
→ monitor
→ rollback nếu guardrail fail
```

- [ ] Exact code/page/input-schema/data/scoring/stack/link versions và manifest checksum.
- [ ] Critical freshness 100%, noncritical ≥98% hoặc phần chưa đạt bị ẩn/gắn trạng thái.
- [ ] Test report/reviewer; 0 P0/P1.
- [ ] Canonical outputs qua Valid Stack Floor; component/reason/evidence audit được.
- [ ] Direct/router branch, disclosure, destination, event-once và fallback pass.
- [ ] Không secret/PII trong public files/log/events.
- [ ] Backup restore + rollback target đã chạy.
- [ ] Live evidence, alert owner/window và Release/Decision Log.

## 16. Khi nào cần kiến trúc lớn hơn?

Chỉ cân nhắc database, queue, services, vector store, agent orchestration hoặc ML khi có bottleneck đo được:

- CSV có conflict/ghi đồng thời thật hoặc referential integrity không còn kiểm soát được.
- Jobs vượt SLA/rate limits; server-side tracking có lợi ích đã chứng minh.
- Nhiều markets/languages đã qua commercial gate.
- Recommendation có matured clean data nhưng deterministic rules không giải quyết được vấn đề đã đo.
- Reliability/security requirement vượt khả năng một static app nhỏ.

Trước đó, complexity làm tăng incident, chi phí và thời gian review nhanh hơn giá trị. Nâng kiến trúc là một change request có budget, threat model, migration/export/restore test và rollback; không phải phần thưởng vì đã đến Day 90.

