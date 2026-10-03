# Điều hành chất lượng

Web app một trang để theo dõi chất lượng các bưu cục. Mỗi ngày tải file xuất từ hệ thống lên, app lưu vào một thư mục trên máy và cho xem số liệu theo ngày, tuần, tháng, quý hoặc lũy kế năm.

## Cách dùng

1. Mở `index.html` bằng **Microsoft Edge hoặc Google Chrome trên máy tính** (Firefox, Safari và trình duyệt trên điện thoại/máy tính bảng chưa hỗ trợ ghi thư mục).
2. Bấm **Chọn thư mục lưu dữ liệu**, chọn một thư mục riêng, ví dụ `D:\DieuHanhChatLuong` hoặc một thư mục trong OneDrive để có bản sao trên cloud. Trình duyệt hỏi quyền đọc ghi, bấm cho phép.
3. Bấm **Tải dữ liệu**, chọn file xuất từ hệ thống (Excel, CSV hoặc JSON, chọn được nhiều file một lúc).
4. Kiểm tra màn hình **Kiểm tra trước khi lưu** rồi bấm Lưu.

Lần sau mở app, trình duyệt nhớ thư mục; chỉ cần bấm "Mở lại thư mục" để cấp lại quyền.

## Cách lưu trên đĩa

```
<thư mục đã chọn>/
├─ cau-hinh.json        mẫu nhận diện từng sheet: vai trò từng cột, cách tính, tỷ lệ
├─ danh-muc.json        danh sách các ngày đã lưu
├─ goc/2026-09/         file gốc, giữ nguyên: "2026-09-29 - BaoCaoNgay_29-09-2026.xlsx"
└─ du-lieu/2026-09/     bản đã chuẩn hóa theo ngày: "2026-09-29.json"
```

- Tải lại cùng một ngày thì ghi đè ngày đó (file gốc cũ của ngày đó bị thay).
- `du-lieu/*.json` là văn bản thường, mỗi dòng dữ liệu một dòng, mở được bằng Notepad.
- Mất `danh-muc.json` thì vào **Dữ liệu đã lưu → Quét lại thư mục** để dựng lại từ `du-lieu/`.

## Cách app hiểu file

- **Ngày dữ liệu**: lấy từ tên tệp (`29-09-2026`, `2026-09-29`, `29092026`), nếu không có thì lấy ngày lớn nhất trong các cột ngày, sửa được trước khi lưu.
- **Mỗi sheet** là một mục ở cột trái, nhận theo tên sheet. File CSV/JSON dùng tên tệp đã bỏ phần ngày.
- **Kiểu sheet**: số phát sinh trong ngày, hoặc số dư tại thời điểm (danh sách tồn).
- **Vai trò cột**:

| Vai trò | Khi xem nhiều ngày |
|---|---|
| Chỉ tiêu cộng dồn | Tổng các ngày, kèm trung bình/ngày |
| Chỉ tiêu số dư (tồn) | Số của ngày cuối kỳ, kèm bình quân/ngày. Không cộng dồn |
| Chỉ tiêu tính trung bình | Trung bình trên các dòng, kèm giá trị lớn nhất |
| Tỷ lệ (tự khai báo) | Tổng tử số / tổng mẫu số của cả kỳ × 100 |
| Bưu cục | Đơn vị để tổng hợp và lọc |
| Nhóm để lọc, Mã định danh, Ngày giờ, Ghi chú, Bỏ qua | Lọc, đếm, hiển thị hoặc không lưu |

Lần đầu gặp một sheet, app đoán vai trò theo kiểu dữ liệu và tên cột. Bạn sửa và lưu thì mẫu được ghi vào `cau-hinh.json`; các ngày sau tự áp dụng và báo nếu file thiếu cột hoặc có cột mới.

## Xem theo kỳ

Ngày, tuần (thứ Hai đến Chủ nhật), tháng, quý, lũy kế năm, hoặc khoảng tùy chọn. Mỗi kỳ so với kỳ trước (lũy kế năm so với cùng kỳ năm trước). Bảng **Tổng hợp theo bưu cục** có cột tổng/cuối kỳ và tùy chọn thêm: trung bình/ngày, ngày cao nhất, ngày thấp nhất, kỳ trước, % thay đổi. Ngày thiếu dữ liệu được báo ở thanh chọn kỳ và đánh dấu vàng trên biểu đồ.

## Thư viện

`vendor/xlsx.full.min.js` là SheetJS Community Edition 0.18.5 (Apache 2.0, xem `vendor/xlsx.LICENSE`), để app đọc Excel không cần mạng. Nếu thiếu tệp này, app tự tải từ cdnjs.

## Việc tiếp theo

- Tạo báo cáo tuần/tháng/quý cho Ban Giám đốc: xuất PDF và PowerPoint chỉnh sửa được.
- Cảnh báo đỏ theo ngưỡng.
