# Stack Rules

## 1. Identity và quyền duyệt

- Stack rules version:
- Scoring version tương thích:
- Input schema version tương thích:
- Data schema version tương thích:
- Link Registry version tương thích:
- Owner:
- Reviewer:
- Approved at:
- Effective at:
- Rollback version:
- Status: `DRAFT / APPROVED / RETIRED`

Mọi số trong file này là rule/threshold nội bộ của một version, không phải benchmark ngành. Chỉ thay prospective bằng version mới + Decision Log; không sửa hồi tố để làm test pass.

## 2. Input contract

- Market được hỗ trợ:
- Currency được hỗ trợ:
- Billing preference values:
- `monthly_budget` so với tổng monthly equivalent thế nào:
- `max_upfront_payment` so với actual/upfront charge thế nào:
- Team-size range:
- Pricing model/price unit/seat formula được phép:
- Must-have need groups:
- Nice-to-have need groups:
- Existing tools/incompatibility inputs:

## 3. Valid Stack Floor — điều kiện bắt buộc trước khi xếp hạng

- [ ] Mọi product/plan đều `ACTIVE` và phục vụ market đã chọn.
- [ ] 100% must-have need groups có ít nhất một product được evidence hỗ trợ.
- [ ] Mọi hard constraint của từng product/plan đều pass.
- [ ] Actual charge không vượt `max_upfront_payment`.
- [ ] Monthly equivalent của toàn stack không vượt monthly budget.
- [ ] 100% critical fields có approved evidence và đang trong freshness SLA.
- [ ] Critical conflict/unknown bằng 0.
- [ ] Không có incompatible pair.
- [ ] Không có duplicate product/function ngoài allowlist bổ sung.
- [ ] Số công cụ không vượt giới hạn đã duyệt.
- [ ] Không dùng commission hoặc Commercial Score để đạt floor hay xếp hạng.

Minimum product User Fit nếu dự án dùng:

- Giá trị:
- Lý do/evidence:
- Nếu không đặt floor, ghi `NONE`; không tự dùng một số mặc định.

## 4. Need groups và coverage

| Need group | Bắt buộc/tùy chọn | Trọng số nhu cầu | Feature keys chứng minh coverage | Cho phép nhiều product? | Notes |
|---|---|---:|---|---|---|
|  |  |  |  |  |  |

## 5. Duplicate, overlap và compatibility

| Rule ID | Product/category A | Product/category B | `ALLOW / BLOCK / PENALIZE` | Adjustment | Evidence/reason |
|---|---|---|---|---:|---|
|  |  |  |  |  |  |

Quy tắc mặc định: tối đa một product cho một need group. Chỉ cho phép hai product khi rule `ALLOW` mô tả chức năng bổ sung, chi phí không bị đếm trùng và người dùng được giải thích trade-off.

## 6. Stack Fit

Ghi công thức cụ thể trước khi chạy. Gợi ý có thể audit:

```text
Base Stack Fit
= weighted mean của Product User Fit theo need-group weight

Stack Fit
= clamp(Base Stack Fit + approved integration/overlap/complexity adjustments, 0, 100)
```

- Cách xử lý một product phủ nhiều need groups:
- Cách tránh nhân đôi điểm:
- Stack adjustment allowlist:
- Giới hạn tổng adjustment:
- Cách làm tròn:

`Fit Score` là điểm tương đối trong candidate set, dataset, scoring version và stack-rules version hiện tại. Nó không phải xác suất mua thành công, benchmark thị trường hay chất lượng tuyệt đối.

## 7. Ba output công khai

### Best Fit

Valid stack có `Stack Fit` cao nhất.

### Cheapest Qualified

Valid stack có tổng monthly equivalent thấp nhất nhưng vẫn qua toàn bộ Valid Stack Floor và minimum fit đã khai báo. Không hạ floor để tạo một kết quả rẻ hơn.

### Alternative

Valid stack khác Best Fit ở ít nhất một product và có trade-off có ý nghĩa đã được reason code giải thích. Không dùng Alternative như chỗ đặt sản phẩm hoa hồng cao hơn.

### Maximum Automation — tùy chọn

Chỉ bật khi có rubric automation 0–5, evidence keys, freshness rule và Golden Tests đã duyệt cho mọi candidate liên quan. Nếu thiếu bất kỳ phần nào, output này phải được ẩn; không suy từ marketing copy hoặc LLM. Nhãn này không cấp L3 hoặc change authority cho runtime/job.

## 8. Tie order

1. Evidence Confidence cao hơn.
2. Tổng monthly equivalent thấp hơn.
3. Actual charge/upfront thấp hơn.
4. Ít công cụ hơn.
5. Stable stack ID.

Commercial Score/commission không được dùng để phá hòa public.

## 9. No-result và degradation

- Không valid stack:
- Chỉ có stack dùng data stale/unknown:
- Currency/price không so được:
- Một need group không có candidate:
- Engine/rules/data version mismatch:

Không tự nới hard constraint. Chỉ gợi ý người dùng tự thay một input, kèm ảnh hưởng dự kiến.

## 10. Acceptance tests

- [ ] Happy path.
- [ ] Budget đúng biên.
- [ ] Annual equivalent hợp ngân sách nhưng upfront vượt trần.
- [ ] Missing/stale/conflict critical evidence.
- [ ] Một product phủ nhiều need groups.
- [ ] Duplicate và incompatible pair.
- [ ] Cheapest không qua floor bị loại.
- [ ] Alternative có trade-off thật.
- [ ] Non-affiliate product có thể đứng đầu.
- [ ] Commission thay đổi không làm output đổi.
- [ ] Maximum Automation bị ẩn khi rubric/evidence thiếu.
- [ ] Cùng input + exact versions tạo cùng output và reason codes.

## 11. Sign-off

- Golden Test reference:
- Component Score sample reference:
- Decision ID:
- Approved/Rejected:
- Reviewer/date:

