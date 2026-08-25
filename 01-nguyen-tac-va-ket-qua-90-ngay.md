# 01 — Nguyên tắc, phạm vi và kết quả 90 ngày

## 1. Mô hình bằng ngôn ngữ đời thường

Bạn không bán con bot. Bạn xây một công cụ miễn phí giúp người dùng ra quyết định mua phần mềm tốt hơn. Nếu người dùng tự tìm thấy công cụ, nhận được gợi ý hữu ích, bấm một liên kết đã công bố là link affiliate và mua sản phẩm, chương trình affiliate có thể ghi nhận hoa hồng cho bạn.

```text
Nhu cầu thật
→ Utility hữu ích
→ Kết quả có căn cứ
→ Click affiliate được công bố
→ Order
→ Valid Order
→ Final Commission
→ Payment
```

Bot làm phần lặp lại: phát hiện dữ liệu cũ, nháp cập nhật, kiểm tra liên kết, tổng hợp KPI và cảnh báo. Con người chịu trách nhiệm cho tính đúng, chính sách, tiền và quyết định rủi ro.

## 2. Sản phẩm đầu tiên đã khóa phạm vi

### AI/SaaS Stack Builder

Người dùng nhập:

- Vai trò hoặc loại hình công việc.
- Mục tiêu chính.
- Quy mô cá nhân/nhóm.
- Ngân sách tháng.
- Cách muốn thanh toán: chỉ trả theo tháng hay chấp nhận trả trước theo năm; mức chi trả trước tối đa.
- Nhu cầu: SEO, nội dung, email, analytics, automation, hình ảnh/video…
- Mức ưu tiên: rẻ nhất, cân bằng, hay tự động hóa cao.

Hệ thống trả về tối đa ba phương án:

1. Phù hợp tổng thể.
2. Chi phí thấp nhất vẫn đáp ứng điều kiện tối thiểu.
3. Một phương án thay thế khác biệt có ý nghĩa, chẳng hạn ít công cụ hơn hoặc đánh đổi khác. Chỉ gắn nhãn “Tự động hóa cao nhất” khi đã có rubric `automation_coverage` và evidence được duyệt; nếu chưa có thì không hiển thị nhãn đó.

Mỗi kết quả phải cho biết:

- Tổng chi phí và đồng tiền.
- Số tiền thực trả mỗi kỳ và mức quy đổi theo tháng; không đánh đồng hai số.
- Nhu cầu nào được/không được đáp ứng.
- Lý do chọn từng sản phẩm.
- Giả định và giới hạn của kết quả.
- Ngày dữ liệu được kiểm tra lần cuối.
- Lựa chọn không có affiliate nếu lựa chọn đó phù hợp hơn.
- Công bố quan hệ affiliate ngay gần lời đề xuất/nút bấm.

### Không làm trong 90 ngày

- Không mở thêm ngách thứ hai.
- Không làm Data/API Bot để bán dữ liệu.
- Không làm marketplace tài sản số.
- Không tạo hàng trăm trang SEO.
- Không chạy quảng cáo trả phí.
- Không tự động nhắn tin, bình luận hoặc rải link.
- Không scrape nguồn chưa kiểm tra điều khoản/quyền sử dụng.
- Không để bot tự publish hoàn toàn.
- Không lưu thông tin nhạy cảm, tài khoản ngân hàng hay khóa bí mật trong prompt/tài liệu công khai.

## 3. Định nghĩa “tự vận hành” đúng mức

| Mức | Bot được làm | Con người phải làm |
|---|---|---|
| L0 — Thủ công | Không có | Làm và kiểm tra mọi bước |
| L1 — Trợ lý | Tạo nháp, gợi ý, phát hiện lỗi | Xác minh và bấm chấp thuận từng thay đổi |
| L2 — Chạy lịch có duyệt | Chạy kiểm tra/refresh và tạo change set | Duyệt trước khi dữ liệu công khai thay đổi |
| L3 — Tự động giới hạn | Tự xử lý việc nhỏ, đảo ngược được, có log/rollback | Xem cảnh báo và audit theo mẫu |
| L4 — Tự quyết | Không triển khai trong 90 ngày | Quyết định tiền, pháp lý, payout, chính sách, claim |

Đích Day 90 mặc định là **L2** cho cập nhật dữ liệu. L3 không bắt buộc; chỉ xét như stretch/post-90 cho đúng một tác vụ an toàn, đảo ngược được sau khi đủ số run, thời gian quan sát, failure cases, log và rollback ở Gate 8. Không cần và không nên đạt L4.

Các mức L0–L4 nói về **quyền thay đổi dữ liệu/cấu hình và workflow nền**, không nói về runtime đã được phê duyệt. Utility deterministic và một link/redirect configuration đã QA có thể phục vụ request tự động từ ngày launch; bot vẫn không được tự thay scoring, offer, destination hay nội dung public.

## 4. Ba tầng thành công

### Tầng A — Thành công về hệ thống

Đạt khi toàn bộ điều sau đúng:

- Website public và dùng được trên điện thoại.
- Utility xử lý đúng các ca kiểm thử đã định.
- 100% sản phẩm hiển thị có nguồn và ngày xác minh.
- Affiliate disclosure, privacy notice và trang phương pháp chấm điểm được hiển thị.
- Tracking `utility_start`, `utility_complete`, `result_view`, `product_outbound_click` đã được thử thật; `affiliate_click` chỉ phát sinh/được suy ra khi `link_type=AFFILIATE`.
- Nếu affiliate link đã active, có quy trình đối soát order/commission bằng báo cáo của network. Nếu đang soft-launch bằng link chính thức không affiliate, trạng thái là `NOT_APPLICABLE_YET` kèm kế hoạch test đối soát sau khi được duyệt chương trình.
- Không còn lỗi nghiêm trọng P0/P1.

### Tầng B — Thành công về tín hiệu thị trường

Một hoặc nhiều tín hiệu có thật:

- Người dùng mục tiêu hoàn thành utility.
- Có click affiliate hợp lệ từ traffic không phải chính bạn.
- Có query/impression liên quan trong Search Console.
- Có phản hồi cho thấy utility giúp ra quyết định.

### Tầng C — Thành công về doanh thu

Theo độ chắc chắn tăng dần:

```text
Order ghi nhận
< Valid Order
< Final Commission
< Payment đã nhận
< Lợi nhuận dương sau chi phí
```

North Star dài hạn là **lợi nhuận trên 1.000 qualified sessions đã mature**, không phải số bài hay pageview. Trong MVP, một `Qualified Visit` chỉ là tên hiển thị thân thiện cho một session đạt điều kiện; không được suy diễn đó là một người duy nhất.

## 5. Vai trò — ai chịu trách nhiệm việc gì?

| Công việc | Bot | Bạn | Chuyên gia khi cần |
|---|:---:|:---:|:---:|
| Tạo danh sách nguồn ứng viên | R | A | — |
| Đọc và lưu bằng chứng từ nguồn | R | A | — |
| Xác nhận điều khoản cho phép sử dụng dữ liệu | Hỗ trợ | A | C |
| Tạo nháp dữ liệu/nội dung | R | A | — |
| Claim về trải nghiệm trực tiếp | Không | A nếu thực sự trải nghiệm | — |
| Phê duyệt publish | Không | A | C nếu rủi ro |
| Tạo link tracking/router | R | A | — |
| Chấp nhận điều khoản affiliate | Không | A | C nếu chưa rõ |
| KYC, payout, thuế | Không | A | C |
| Chi tiền/nâng ngân sách | Không | A | — |
| Báo cáo KPI/đánh dấu bất thường | R | A | — |
| Gỡ trang/link khi có sự cố | R theo rule an toàn | A | C nếu tranh chấp |

Ký hiệu: `R` = thực hiện; `A` = chịu trách nhiệm cuối; `C` = tham vấn.

Trong Solo Mode, một người có thể đội toàn bộ các “mũ” owner. Tên vai trò chỉ giúp nhớ trách nhiệm nào phải được kiểm tra; không có nghĩa bạn cần thuê một đội. Chỉ gọi chuyên gia khi trigger pháp lý, thuế, bảo mật hoặc dữ liệu thật sự xuất hiện.

## 6. Ngân sách và quyền hạn

Trước khi bắt đầu, đặt bốn con số:

```text
BUDGET_SETUP_MAX = chi một lần tối đa
BUDGET_MONTHLY_MAX = chi phí lặp lại tối đa/tháng
BUDGET_EXPERIMENT_MAX = chi phí tối đa cho một thử nghiệm
HUMAN_APPROVAL_THRESHOLD = mọi khoản chi từ mức này phải xác nhận lại
```

Quy tắc cứng:

- Bot không được tự nhập thẻ, mua domain, nâng gói, ký điều khoản hoặc thay payout.
- Mọi subscription phải có owner, ngày gia hạn, cách hủy và lý do tồn tại.
- Không tính “thời gian của bạn = 0” khi đánh giá dài hạn; theo dõi cả giờ vận hành.
- Nếu vượt 80% ngân sách tháng, dừng job không quan trọng và báo động vàng.
- Nếu dự kiến vượt 100%, dừng mọi job có chi phí biến đổi; chỉ giữ website và cảnh báo.

## 7. Definition of Done chung

Một nhiệm vụ chỉ là `PASS` khi có đủ năm thứ:

1. **Đầu ra:** file, URL, bản ghi hoặc ảnh chụp tồn tại.
2. **Bằng chứng:** nguồn/test/log chứng minh kết quả.
3. **Owner:** biết ai chịu trách nhiệm.
4. **Ngày kiểm tra:** không dùng từ “mới nhất” mà thiếu timestamp.
5. **Bước tiếp:** đã ghi hành động hoặc xác nhận không cần hành động.

“Đã đọc”, “đã hỏi AI”, “có vẻ đúng” hoặc “trang chạy trên máy tôi” chưa phải Definition of Done.

## 8. Quy tắc bằng chứng

### Thứ tự ưu tiên nguồn

1. Trang pricing, feature, affiliate terms hoặc API chính thức của nhà cung cấp.
2. Tài liệu/trợ giúp chính thức có ngày cập nhật.
3. Dashboard/tài khoản của chính bạn.
4. Nguồn thứ cấp uy tín để phát hiện vấn đề, không dùng làm nguồn duy nhất cho giá/điều khoản.
5. Nội dung cộng đồng chỉ là tín hiệu cần xác minh.

### Mức tin cậy dữ liệu

- `A`: nguồn chính thức, rõ ràng, kiểm tra trong chu kỳ yêu cầu.
- `B`: nguồn chính thức nhưng có điểm chưa rõ/khác vùng.
- `C`: nguồn thứ cấp hoặc chưa xác minh; không dùng cho recommendation công khai.
- `BLOCKED`: nguồn mâu thuẫn, điều khoản không rõ hoặc không có quyền sử dụng.

### Chu kỳ làm mới gợi ý

| Dữ liệu | Chu kỳ ban đầu | Khi phải kiểm tra ngay |
|---|---:|---|
| Giá/gói | 14 ngày | Có cảnh báo thay đổi hoặc người dùng báo sai |
| Tính năng chính | 30 ngày | Product release làm thay đổi recommendation |
| Affiliate terms/rate/cookie | 30 ngày | Email/network thông báo hoặc link lỗi |
| Chính sách pháp lý/nền tảng | 90 ngày | Trước launch/scale hoặc có thông báo mới |
| Claim trải nghiệm | Mỗi lần publish | Sửa nội dung hoặc phạm vi trải nghiệm thay đổi |

Đây là chu kỳ vận hành, không phải khẳng định dữ liệu chỉ thay đổi theo lịch.

## 9. Nhật ký quyết định bắt buộc

Mỗi quyết định lớn phải ghi:

```text
Decision ID:
Ngày:
Quyết định:
Bối cảnh:
Các lựa chọn đã xem:
Bằng chứng:
Giả định:
Rủi ro:
Người phê duyệt:
Ngày xem lại:
Kết quả sau xem lại:
```

Các quyết định bắt buộc có log: chọn ngách/ICP, chọn chương trình, chọn công nghệ, thay scoring, publish tự động, tăng ngân sách, thay nguồn dữ liệu và Scale/Kill.

## 10. Tuyên bố kỳ vọng trung thực

- Organic traffic không đến ngay và không được bảo đảm.
- Được nhận vào một affiliate program không bảo đảm conversion hoặc payout.
- Order có thể bị hủy, hoàn tiền hoặc từ chối trước khi thành final commission.
- Search engine và affiliate program có thể đổi chính sách.
- “Tự động” luôn cần monitoring, giới hạn chi phí, log và đường dừng khẩn cấp.
- 90 ngày đầu là giai đoạn xây một vòng học có đo lường, không phải cam kết thu nhập thụ động.

