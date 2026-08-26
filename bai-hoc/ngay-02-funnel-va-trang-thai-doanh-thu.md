# Ngày 2 — Vẽ funnel doanh thu và hiểu các trạng thái tiền

> Thời lượng: 60–90 phút  
> Đầu ra bắt buộc: `Funnel Map v1`  
> Điều kiện hoàn thành: bạn giải thích được vì sao `click`, `order`, `valid`, `final commission` và `payment` là năm trạng thái khác nhau.

## 1. Hôm nay bạn sẽ làm được gì?

Hôm nay bạn sẽ vẽ đường đi của một người dùng từ lúc chưa biết website của bạn đến lúc (nếu mọi điều kiện hợp lệ xảy ra) khoản hoa hồng thực sự được trả.

Mục tiêu không phải là dự báo thu nhập. Mục tiêu là không tự lừa mình bằng những chỉ số “trông có vẻ tốt” nhưng chưa phải tiền. Một click là tín hiệu quan tâm; nó không phải đơn hàng. Một đơn hàng có thể bị hủy; nó không phải commission cuối cùng. Một commission cuối cùng vẫn có thể chưa được trả.

Đến cuối buổi, bạn có một sơ đồ đơn giản để biết:

- Mỗi giai đoạn đang đo điều gì.
- Dữ liệu nào là dữ liệu bạn tự đo, dữ liệu nào chỉ affiliate program mới biết.
- Con người cần kiểm tra gì và automation có thể làm gì.
- Khi một con số giảm, bạn nên xem lại phần nào thay vì đoán bừa.

---

## 2. Funnel là gì?

**Funnel** (phễu) là cách mô tả một chuỗi bước: số người thường giảm dần ở mỗi bước vì không phải ai cũng muốn, đủ điều kiện hoặc hoàn tất bước tiếp theo.

Trong dự án này, funnel cơ bản là:

```text
impression → visit → utility_start → utility_complete → result_view
→ outbound_click → order → valid → final_commission → payment
```

Bạn có thể đọc nó theo tiếng Việt:

```text
Được nhìn thấy → vào website → bắt đầu dùng utility → hoàn thành utility
→ xem kết quả → bấm sang sản phẩm → có đơn được báo → đơn hợp lệ
→ hoa hồng chốt cuối → tiền được thanh toán
```

Không phải dự án nào cũng nhìn thấy toàn bộ chuỗi. Ví dụ trong giai đoạn chưa được duyệt affiliate program, bạn chỉ có thể đo đến `outbound_click` sang product page. Điều đó bình thường, miễn là bạn ghi rõ trạng thái `NOT_APPLICABLE_YET`, không gọi click là doanh thu.

---

## 3. Giải thích từng trạng thái bằng ngôn ngữ đơn giản

| Trạng thái | Điều đã xảy ra | Bạn thường tự đo được? | Chưa được phép kết luận |
|---|---|---:|---|
| `impression` | Một kết quả/trang của bạn được hiển thị ở nơi có thể thấy | Có, nếu nền tảng báo | Người đó đã đọc hoặc cần sản phẩm |
| `visit` | Có phiên truy cập website | Có | Họ là đúng ICP hoặc đã dùng utility |
| `utility_start` | Họ bắt đầu nhập thông tin vào utility | Có | Họ đã nhận được gợi ý |
| `utility_complete` | Họ gửi đủ input để utility tính kết quả | Có | Họ tin tưởng hoặc sẽ bấm link |
| `result_view` | Kết quả gợi ý đã hiện cho họ | Có | Họ đã xem kỹ mọi gợi ý |
| `outbound_click` | Họ bấm từ website sang trang sản phẩm | Có | Đã có đơn hoặc có commission |
| `order` | Affiliate program báo một đơn/lead | Thường không; tùy program | Đơn sẽ được giữ lại |
| `valid` | Đơn qua điều kiện cơ bản của program | Thường không | Hoa hồng đã chốt hoặc đã trả |
| `final_commission` | Khoản commission không còn nằm trong thời gian reversal/duyệt | Thường không | Tiền đã về tài khoản/payout method |
| `payment` | Program/network thực sự thanh toán | Chỉ đối soát từ payout statement | Dự án chắc chắn có lợi nhuận sau toàn bộ chi phí |

### 3.1. Ba chỗ dễ nhầm nhất

**Click không phải order.** Người dùng có thể bấm để xem giá, rồi đóng tab, không phù hợp, hoặc mua sau khi cookie hết hạn.

**Order không chắc là valid.** Đơn có thể bị hủy, refund, gian lận, không đúng điều kiện chương trình hoặc bị từ chối vì lý do hợp lệ theo terms.

**Final commission không hoàn toàn giống payment.** Một network có thể có ngưỡng thanh toán, lịch trả tiền hoặc thông tin payout chưa hoàn tất. Vì vậy tài chính phải được đối soát bằng payout statement; không suy ra từ dashboard một cách mù quáng.

---

## 4. Ví dụ xuyên suốt: 100 người bắt đầu funnel

Giả sử trong một tuần, bạn có các số liệu **minh họa hư cấu** sau. Đây không phải benchmark, mục tiêu hay dự báo.

| Bước | Số lượng | Cách hiểu đúng |
|---|---:|---|
| `impression` | 1.000 | Nội dung/trang có cơ hội được nhìn thấy 1.000 lần |
| `visit` | 100 | 100 phiên vào website; không phải 100 khách mua |
| `utility_start` | 35 | 35 phiên bắt đầu nhập dữ liệu |
| `utility_complete` | 20 | 20 phiên hoàn thành input |
| `result_view` | 20 | 20 phiên được nhìn thấy kết quả |
| `outbound_click` | 6 | 6 lượt bấm sang product page |
| `order` | 1 | Program báo 1 đơn, nhưng còn chờ |
| `valid` | 1 | Đơn không bị từ chối/refund trong giai đoạn kiểm tra |
| `final_commission` | 1 | Commission được chốt là 12 USD |
| `payment` | 0 | Chưa đến lịch trả hoặc chưa đạt payment threshold |

Trong ví dụ này, không được viết “dự án đã kiếm 12 USD” khi tiền chưa được thanh toán. Cách ghi đúng là:

> Có một `final_commission` 12 USD, `payment` chưa ghi nhận; revenue cash nhận được hiện là 0 USD.

---

## 5. Dữ liệu nào thuộc quyền kiểm soát của bạn?

Chia funnel thành hai vùng để không thiết kế sai tracking.

```text
Vùng A — Website/utility của bạn
impression → visit → utility_start → utility_complete → result_view → outbound_click

Vùng B — Affiliate program/network/merchant
order → valid → final_commission → payment
```

### Vùng A: dữ liệu first-party của bạn

Bạn thiết kế được sự kiện, tên trường và dashboard. Những thông tin tối thiểu nên lưu ở mức pseudonymous là:

```text
journey_id
timestamp
event_name
utility_version
dataset_version
scoring_version
result_set_id
link_id (chỉ khi có outbound click)
```

Không nhét email, số điện thoại, tên, địa chỉ, nội dung nhạy cảm hoặc mã định danh cá nhân vào tracking chỉ để “đo cho đủ”. Xem thêm [Compliance và quản trị rủi ro](../10-compliance-va-rui-ro.md) trước khi thu thập bất cứ dữ liệu nào.

### Vùng B: dữ liệu phía affiliate

Program, network hoặc merchant thường là nguồn sự thật cho order/commission/payout. Bạn không được tự tạo order record từ click log; bạn chỉ có thể **đối soát** record mà họ đã báo, trong phạm vi terms cho phép.

Trong giai đoạn đầu, mục tiêu của bạn là lưu được mapping an toàn:

```text
link_id → program → product → checked date → link status
```

Không dùng sub-ID chứa trực tiếp dữ liệu cá nhân. Đọc terms của từng program trước khi bật tracking parameter hoặc router.

---

## 6. Bài thực hành từng bước: tạo Funnel Map v1

Mở một tài liệu mới tên `funnel-map-v1.md`. Bạn có thể dùng Markdown, Google Docs, Notion hoặc giấy; điều quan trọng là có ngày, owner và version.

### Bước 1 — Vẽ chuỗi chính (10 phút)

Sao chép chuỗi sau vào tài liệu:

```text
impression
→ visit
→ utility_start
→ utility_complete
→ result_view
→ outbound_click
→ order
→ valid
→ final_commission
→ payment
```

Vẽ một mũi tên bên dưới từ `outbound_click` sang `no_order / unknown`. Đây là nhánh bình thường, không phải lỗi.

```text
outbound_click
├── order
└── no_order / unknown
```

### Bước 2 — Viết định nghĩa đạt cho từng event bạn tự đo (15 phút)

Không dùng định nghĩa mơ hồ như “người dùng tương tác”. Viết điều kiện có thể kiểm thử.

Ví dụ:

| Event | Khi nào được ghi? | Khi nào không ghi? |
|---|---|---|
| `utility_start` | Người dùng thay đổi input đầu tiên hoặc bấm “Bắt đầu” | Chỉ tải trang nhưng không tương tác |
| `utility_complete` | Input bắt buộc hợp lệ và engine chạy | Thiếu budget/must-have hoặc form lỗi |
| `result_view` | Kết quả render thành công với version đã duyệt | Engine lỗi hoặc không có kết quả hợp lệ |
| `outbound_click` | Người dùng chủ động bấm CTA product/link đã duyệt | Link bị disabled, lỗi hoặc auto-redirect |

**Quy tắc quan trọng:** không tự redirect để tăng click và không ghi event click nếu link không mở được. Cả hai làm bẩn số liệu và có thể trái terms.

### Bước 3 — Đánh dấu nguồn sự thật (10 phút)

Thêm bảng này vào Funnel Map:

| Trạng thái | Nguồn sự thật | Owner kiểm tra | Nhịp kiểm tra |
|---|---|---|---|
| `visit` đến `outbound_click` | Analytics/log của website | Owner dự án | Hằng tuần |
| `order` đến `final_commission` | Dashboard/report chính thức của program/network | Owner dự án | Theo lịch program, tối thiểu hằng tuần sau launch |
| `payment` | Payout statement/transaction record | Owner dự án | Mỗi kỳ payout |

Nếu chưa tham gia program, điền `NOT_APPLICABLE_YET` cho ba dòng cuối và ghi điều kiện để kích hoạt: “Chỉ thay bằng data thật sau khi program được duyệt và terms cho phép.”

### Bước 4 — Chọn chỉ ba tỷ lệ ban đầu (15 phút)

Ngày 2 không cần dashboard phức tạp. Chỉ định nghĩa ba tỷ lệ sau:

```text
Utility completion rate = utility_complete / utility_start
Outbound click rate     = outbound_click / result_view
Order rate              = reported_order / outbound_click
```

Luôn lưu tử số và mẫu số cùng nhau. Đừng chỉ báo “completion rate 50%” mà không ghi đó là 1/2 hay 500/1.000; hai trường hợp có độ tin cậy rất khác nhau.

Ví dụ tính tay:

```text
20 utility_complete / 35 utility_start = 57,1%
6 outbound_click / 20 result_view      = 30%
1 order / 6 outbound_click             = 16,7%
```

Tỷ lệ order chỉ được ghi khi program thực sự báo order. Nếu không có order data, ghi `NOT_AVAILABLE`, không tự thay bằng “click rate”.

### Bước 5 — Viết ba câu chẩn đoán, không kết luận quá mức (15 phút)

Với mỗi điểm rơi của funnel, viết một giả thuyết cần kiểm tra. Không tuyên bố nguyên nhân khi chưa có bằng chứng.

| Điểm quan sát | Diễn giải quá mức | Cách viết đúng |
|---|---|---|
| Nhiều visit, ít utility start | “Người dùng ghét utility.” | “Cần kiểm tra CTA, page intent, tốc độ tải và mức khớp giữa query với trang.” |
| Nhiều utility complete, ít outbound click | “Sản phẩm không tốt.” | “Cần kiểm tra độ tin cậy của kết quả, giá hiển thị, disclosure, CTA và mức phù hợp của recommendation.” |
| Có click, không có order | “Affiliate không hiệu quả.” | “Chưa đủ data hoặc cần kiểm tra market fit, link, terms, product page, attribution và thời gian quan sát.” |

### Bước 6 — Chốt những gì bot không được tự suy diễn (10 phút)

Thêm khung này cuối Funnel Map:

```text
Bot được phép:
- Ghi event kỹ thuật đã định nghĩa.
- Tính tỷ lệ từ dữ liệu đã có.
- Báo link lỗi hoặc data thiếu để con người xem.

Bot không được phép:
- Tự gán một click thành order/commission.
- Tự đổi trạng thái final/payment.
- Tự bấm link, tự mua hoặc tạo traffic.
- Tự thay terms, payout, sub-ID hoặc quyết định attribution.
- Tự kết luận nguyên nhân kinh doanh từ mẫu nhỏ.
```

---

## 7. Bài tập tự kiểm

Trả lời trước khi xem đáp án:

1. Một người bấm affiliate link nhưng chưa mua. Funnel dừng ở đâu?
2. Dashboard program báo một order nhưng đơn còn trong thời gian refund. Bạn ghi là gì?
3. `outbound_click / result_view` giảm mạnh. Đây có chứng minh ranking sai không?
4. Khi chưa được duyệt affiliate program, bạn nên lưu loại dữ liệu gì và không nên khẳng định điều gì?
5. Vì sao cần lưu `journey_id` giả danh thay vì email trong event log?

### Đáp án tham khảo

1. Dừng ở `outbound_click`; `order` là unknown/no order cho đến khi nguồn chính thức báo.
2. Ghi `order` hoặc `pending` theo trạng thái chính thức, chưa ghi `valid`, `final_commission` hay `payment`.
3. Không. Nó chỉ là tín hiệu cần điều tra CTA, trust, output, giá, disclosure, intent và lỗi kỹ thuật.
4. Lưu event website, `link_id`, product, source/version và trạng thái `NOT_APPLICABLE_YET`; không gọi click là affiliate sale hay revenue.
5. `journey_id` cho phép nối các event trong một hành trình mà không thu thập trực tiếp định danh cá nhân không cần thiết.

---

## 8. Checklist hoàn thành Ngày 2

- [ ] Tôi đã tạo `Funnel Map v1` có ngày, owner và version.
- [ ] Sơ đồ có đủ chuỗi từ `impression` đến `payment`.
- [ ] Tôi phân biệt rõ click, order, valid, final commission và payment.
- [ ] Tôi đã đánh dấu phần website và phần thuộc nguồn sự thật của affiliate program.
- [ ] Tôi đã định nghĩa khi nào ghi `utility_start`, `utility_complete`, `result_view`, `outbound_click`.
- [ ] Tôi đã viết ba tỷ lệ, mỗi tỷ lệ có tử số/mẫu số.
- [ ] Trạng thái không có dữ liệu được ghi `NOT_AVAILABLE` hoặc `NOT_APPLICABLE_YET`, không bị biến thành 0 doanh thu hay 0 order theo suy đoán.
- [ ] Tôi đã ghi rõ những việc automation không được làm.

## 9. Lỗi thường gặp và cách sửa

| Lỗi | Tại sao sai | Cách sửa |
|---|---|---|
| Gọi click là conversion | Click chỉ là chuyển hướng | Gọi đúng là `outbound_click`; đợi nguồn chính thức cho order |
| Gộp pending và final commission | Có thể có refund/reversal/duyệt chậm | Giữ trạng thái nguyên vẹn trong ledger |
| Chỉ nhìn phần trăm | 50% của 2 người và 50% của 2.000 người khác nhau | Luôn ghi tử số/mẫu số, khoảng thời gian và nguồn |
| Đổ lỗi cho một yếu tố | Funnel có nhiều nguyên nhân đồng thời | Viết giả thuyết cần kiểm tra, không viết kết luận |
| Ghi PII vào tracking | Tăng rủi ro privacy mà không cần thiết | Dùng `journey_id` giả danh và inventory dữ liệu |
| Tự động đổi trạng thái payout | Hệ thống của bạn không phải nguồn sự thật | Chỉ đồng bộ/đối soát từ record chính thức đã được duyệt |

## 10. Đầu ra cần lưu

Lưu các mục sau trong thư mục dự án:

```text
02-funnel/funnel-map-v1.md
02-funnel/event-definitions-v1.md
02-funnel/metric-definitions-v1.md
```

Bạn có thể gộp cả ba trong một tài liệu ở giai đoạn này. Khi triển khai tracking về sau, đối chiếu lại với [KPI và unit economics](../07-kpi-va-unit-economics.md) và [Event Contract](../templates/21-event-contract.md).

## 11. Điều kiện để sang Ngày 3

Bạn có thể chuyển sang Ngày 3 khi:

```text
Funnel Map v1 tồn tại
AND mỗi trạng thái có định nghĩa hoặc nguồn sự thật
AND click/order/final/payment không bị gộp làm một
AND ba tỷ lệ cơ bản có công thức rõ
AND giới hạn automation đã được ghi
```

Nếu còn lẫn lộn, hãy lấy ví dụ 100 người ở phần 4, thay số bằng lời và diễn giải lại từng mũi tên. Chưa cần cài analytics hôm nay; bạn chỉ đang thiết kế thứ sẽ được đo sau này.

---

## Ghi chú của bạn

```text
Ngày hoàn thành:
Thời gian thực tế đã dùng:
Điểm funnel tôi hiểu rõ nhất:
Trạng thái tôi còn thấy khó:
Giả định cần kiểm tra ở Ngày 3:
Blocker cụ thể (nếu có):
Trạng thái: NOT_STARTED / IN_PROGRESS / PASS / BLOCKED
```

