# Project Charter — Affiliate Utility Bot

## 1. Thông tin cơ bản

- Project ID:
- Owner:
- Ngày bắt đầu:
- Ngày Day 90:
- Số giờ có thể dành mỗi tuần:
- Ngày/giờ review cố định:
- Chế độ: `STANDARD / LEAN`

## 2. Mục tiêu một câu

> Tôi giúp **[nhóm người dùng]** chọn **[loại công cụ]** khi **[tình huống/ràng buộc]** bằng **[utility/giá trị khác biệt]**.

## 3. ICP và thị trường

- Segment:
- JTBD:
- Vấn đề chính:
- Ngôn ngữ website:
- Quốc gia/thị trường mục tiêu:
- Quốc gia nơi chủ dự án vận hành:
- Những nhóm không phục vụ trong Phase 1:

## 4. Sản phẩm 90 ngày

- Utility duy nhất: AI/SaaS Stack Builder.
- Inputs dự kiến:
- Outputs dự kiến:
- Số sản phẩm: `STANDARD 12–15 / LEAN 8`
- Số decision assets: `STANDARD 5 / LEAN 3` (trust/legal pages tính riêng và vẫn bắt buộc)
- Tracking tối thiểu: `journey_id`, `utility_start`, `utility_complete`, `result_view`, `product_outbound_click`; thêm `affiliate_click` khi có link affiliate.
- Change authority tối đa dự kiến: `L1 / L2 / L3 giới hạn`; mặc định Day 90 là `L2`, L3 chỉ là stretch sau khi qua Gate 8.

## 5. Tiêu chí thành công

### System success

- [ ] Website public/mobile.
- [ ] Golden Tests đạt gate.
- [ ] Critical facts 100% có evidence.
- [ ] Disclosure/privacy/methodology.
- [ ] Funnel events; reconciliation nếu affiliate active, hoặc `NOT_APPLICABLE_YET + test plan` nếu soft-launch non-affiliate.
- [ ] Backup/rollback/incident runbook.

### Market signal mong muốn

- [ ] Người đúng ICP hoàn thành utility.
- [ ] Organic impression/click hoặc direct qualified use.
- [ ] Qualified outbound click; affiliate click nếu chương trình đã active.
- [ ] Feedback cụ thể.

### Revenue signal mong muốn nhưng không bảo đảm trong 90 ngày

- [ ] Order reported.
- [ ] Valid order.
- [ ] Final commission.
- [ ] Payment received.
- [ ] Positive contribution after cost.

## 6. Non-goals

- [ ] Không ngách thứ hai.
- [ ] Không paid ads/spam/outreach bán bot.
- [ ] Không hàng trăm trang AI.
- [ ] Không auto-publish material claims.
- [ ] Không scrape trái quyền/terms/robots.
- [ ] Không tự chi tiền/accept terms/KYC/tax/payout.
- Khác:

## 7. Ngân sách và quyền duyệt

- `BUDGET_SETUP_MAX`:
- `BUDGET_MONTHLY_MAX`:
- `BUDGET_EXPERIMENT_MAX`:
- `HUMAN_APPROVAL_THRESHOLD`:
- Ai được mua/đăng ký:
- Ai được launch/rollback:
- Ai được thay scoring:
- Ai được tăng automation level:

## 8. Red lines và kill conditions

- [ ] Không bịa review/experience/price/feature/commission.
- [ ] Không auto-click/self-purchase/cookie stuffing/fake traffic.
- [ ] Không direct identifier/Google-prohibited PII trong analytics/sub-ID/log; identifier giả danh được inventory và có retention.
- [ ] Không recommendation dựa trên commission.
- [ ] Dừng khi source/program/payout không được phép.
- [ ] Dừng khi lộ secret/sai redirect/sai hard constraint diện rộng.
- Khác:

## 9. Giả định lớn cần kiểm chứng

| ID | Giả định | Bằng chứng cần | Hạn kiểm tra | Trạng thái |
|---|---|---|---|---|
| A-01 |  |  |  | UNKNOWN |

Quy tắc điền theo mốc:

- Day 7 bắt buộc: owner, thời gian, ngân sách trần, non-goals, quyền phê duyệt và thị trường dự kiến. Field chưa biết phải ghi `UNKNOWN + owner + ngày xem lại`, không được đoán.
- Day 14: khóa bản nháp segment/JTBD/problem từ bằng chứng nghiên cứu.
- Day 28: khóa program/link mode, data scope và tracking scope; mọi thay đổi sau đó đi qua Decision Log/Change Request.

## 10. Sign-off

- Owner xác nhận không có bảo đảm doanh thu: `YES / NO`
- Owner xác nhận sẽ duy trì human review sau Day 90: `YES / NO`
- Tên/ngày:

