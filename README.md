# Điều hành chất lượng

Web app một trang: tải file dữ liệu lên là có dashboard ngay. Mọi xử lý diễn ra trong trình duyệt, file không được gửi lên máy chủ nào.

## Bố cục

- Máy tính, laptop, máy tính bảng (rộng từ 768px): cột điều hướng hẹp bên trái, phần nội dung lớn bên phải.
- Điện thoại: điều hướng thu thành dải tab cuộn ngang phía trên.
- Mỗi mục điều hướng là một trang tính (sheet) trong file Excel, hoặc một file CSV/JSON. Chọn được nhiều file một lúc.
- Mỗi mục gồm: hàng bộ lọc (tối đa 6 cột nhóm + Thời gian), 4 ô chỉ số, biểu đồ tròn cơ cấu, biểu đồ cột theo thời gian (hoặc so sánh nhóm nếu không có cột ngày), bảng chi tiết.

## Định dạng hỗ trợ

- CSV / TSV / TXT (tự nhận dấu phân cách `,` `;` tab `|`, UTF-8 có hoặc không BOM)
- Excel `.xlsx`, `.xlsm`, `.xls`, `.ods` (chọn được trang tính nếu file có nhiều sheet)
- JSON (mảng object, mảng mảng, hoặc object chứa một mảng)

File xuất từ app Trạm Quét Mã (repo `leonard/quetma`) đọc được trực tiếp.

## Dashboard tự dựng những gì

- Nhận diện kiểu từng cột: số, ngày giờ, nhóm, văn bản, mã định danh.
  - Số kiểu Việt Nam (`1.250.000`, `12,5`) và kiểu Anh (`1,250,000`, `12.5`) được nhận theo từng cột.
  - Ngày `dd/mm/yyyy`, `yyyy-mm-dd`, có hoặc không kèm giờ, và dạng `HH:mm:ss d/m/yyyy` mà trình duyệt tiếng Việt xuất ra.
  - Cột mã vạch, mã đơn, số điện thoại không bị cộng dồn như số.
- Ô chỉ số: số dòng, tổng và trung bình của tối đa 3 cột số, khoảng thời gian.
- Biểu đồ thanh theo nhóm (top 10, phần còn lại gộp vào "Khác"), chọn được cột nhóm và phép tính (số dòng, tổng, trung bình). Nhấn vào thanh để lọc.
- Biểu đồ theo thời gian, tự chọn mốc giờ / ngày / tháng / năm theo độ dài dữ liệu.
- Bộ lọc chung: khoảng thời gian (tính từ ngày cuối cùng trong file), lọc theo giá trị một cột, tìm kiếm toàn văn.
- Bảng dữ liệu sắp xếp được, phân trang; bảng cấu trúc file.

## Chạy

Mở thẳng `index.html` bằng trình duyệt là dùng được. Cần mạng lần đầu để tải thư viện đọc Excel (SheetJS từ cdnjs); CSV và JSON không cần thư viện.

Muốn có link dùng chung: bật GitHub Pages cho repo (Settings → Pages → Deploy from branch, thư mục gốc).

## Giới hạn đã biết

- Số có dạng mơ hồ như `1.234` (không rõ là nghìn hay thập phân) được hiểu theo kiểu Việt Nam, tức 1234, trừ khi các giá trị khác trong cột cho thấy cột dùng kiểu Anh.
- Ngày `a/b/yyyy` mặc định là ngày/tháng; chỉ chuyển sang tháng/ngày khi trong cột có giá trị mà phần thứ hai lớn hơn 12.
- Dữ liệu nằm trong bộ nhớ trình duyệt; file vài trăm nghìn dòng vẫn chạy nhưng sẽ chậm.
