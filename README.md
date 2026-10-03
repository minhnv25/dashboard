# Điều hành chất lượng

Web app một trang để theo dõi chất lượng các bưu cục. Mỗi ngày tải file xuất từ hệ thống lên, app lưu vào một thư mục trên máy và cho xem số liệu theo ngày, tuần, tháng, quý hoặc lũy kế năm.

## Cách dùng

1. Tạo một thư mục dữ liệu với 4 thư mục con, mỗi ngày bỏ tệp tải từ hệ thống vào đúng thư mục, tên tệp có ngày:

```
<Thư mục dữ liệu>/
├─ TonPhat/          TonPhat_29-09-2026.xlsx, ...   danh sách bưu gửi tồn phát
├─ TonThu/           TonThu_29-09-2026.xlsx, ...    danh sách yêu cầu tồn thu
├─ PhatThanhCong/    PTC_29092026.xlsx, ...         bưu gửi phát thành công trong ngày
└─ ThuThanhCong/     TTC_2026-09-29.xlsx, ...       yêu cầu thu thành công trong ngày
```

   Trong mỗi thư mục con có thể chia thêm thư mục (ví dụ theo tháng). Thư mục con khác tên cũng được đọc, mỗi thư mục là một mục ở cột trái.
2. Mở `index.html` bằng Edge hoặc Chrome trên máy tính, bấm **Liên kết thư mục dữ liệu**, chọn thư mục trên. Trình duyệt nhớ thư mục này.
3. Mỗi lần có tệp mới, bấm **Cập nhật dữ liệu**. App đọc lại thư mục, chỉ đọc tệp mới hoặc vừa sửa.

App chỉ đọc, không sửa, không di chuyển, không xóa tệp của bạn. Trên điện thoại, máy tính bảng, Firefox và Safari, trình duyệt không cho nhớ thư mục: mỗi lần cập nhật phải chọn lại thư mục hoặc các tệp; dữ liệu đã đọc vẫn được nhớ trên thiết bị đó.

## Cách app hiểu và nối các tệp

- **Ngày của tệp**: lấy từ tên tệp (`29-09-2026`, `2026-09-29`, `29092026`); không có thì lấy ngày lớn nhất trong dữ liệu, cuối cùng là ngày sửa tệp.
- **Nhiều tệp cùng ngày** trong một thư mục: gộp lại, trùng mã thì giữ dòng của tệp mới nhất.
- **Nhiều sheet trong một tệp**: các sheet có cùng cấu trúc với thư mục được gộp; sheet khác cấu trúc (ví dụ sheet tổng hợp) bị bỏ qua và được liệt kê ở trang Cập nhật dữ liệu.
- **Vai trò cột** (cộng dồn, số dư, trung bình, bưu cục, nhóm lọc, mã...) được app đoán và có thể sửa ở **Cập nhật dữ liệu → Thiết lập**.
- **Liên kết theo mã**: cùng một mã bưu gửi (mã yêu cầu) xuất hiện ở nhiều thư mục thì dùng chung thông tin. Tệp thiếu cột Bưu cục, Dịch vụ, Lý do tồn... được điền từ thư mục khác; cột được điền có chữ "(liên kết)".
- **Tổng quan** tự tính: mỗi bưu cục mỗi ngày đếm số mã trong từng thư mục. Tồn là số dư (xem nhiều ngày lấy cuối kỳ), thành công là cộng dồn. Tỷ lệ thành công một ngày = thành công / (thành công + tồn); khi xem nhiều ngày chọn được cách tính phần tồn (cộng từng ngày hoặc chỉ cuối kỳ). **Đây là giả định, cần đối chiếu với công thức của đơn vị.**
- **Đối soát** (trên trang Tổng quan): mã vừa tồn vừa thành công cùng ngày; mã ra khỏi danh sách tồn mà không thấy thành công; dòng không xác định được bưu cục.
- Những gì app nhớ trên máy (thư mục đã liên kết, thiết lập, tệp đã đọc) nằm trong bộ nhớ của trình duyệt, không ghi vào thư mục dữ liệu.

## Xem theo kỳ

Ngày, tuần (thứ Hai đến Chủ nhật), tháng, quý, lũy kế năm, hoặc khoảng tùy chọn. Mỗi kỳ so với kỳ trước (lũy kế năm so với cùng kỳ năm trước). Bảng **Tổng hợp theo bưu cục** có cột tổng/cuối kỳ và tùy chọn thêm: trung bình/ngày, ngày cao nhất, ngày thấp nhất, kỳ trước, % thay đổi. Ngày thiếu dữ liệu được báo ở thanh chọn kỳ và đánh dấu vàng trên biểu đồ.

## Thư viện

`vendor/xlsx.full.min.js` là SheetJS Community Edition 0.18.5 (Apache 2.0, xem `vendor/xlsx.LICENSE`), để app đọc Excel không cần mạng. Nếu thiếu tệp này, app tự tải từ cdnjs.

## Việc tiếp theo

- Tạo báo cáo tuần/tháng/quý cho Ban Giám đốc: xuất PDF và PowerPoint chỉnh sửa được.
- Cảnh báo đỏ theo ngưỡng.
