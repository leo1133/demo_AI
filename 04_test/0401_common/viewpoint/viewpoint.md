# Viewpoint

## Input_text

| Kiểm tra hiển thị tại textbox | 1. Thực hiện kiểm tra hiển thị textbox<br>2. Click vào textbox  |  | 1. Hiển thị [tên trường] dưới dạng textbox<br>2. <br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím  |
| --- | --- | --- | --- |
| Kiểm tra khi thực hiện input data | 1. Thực hiện nhập ký tự là chữ <br>( chữ hoa, chữ thường ) |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập ký tự là số  |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập ký tự là ký tự đặc biệt <br>"@#$%^&*()" |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập data có chứa space ở đầu |  | 1. <br>- Hệ thống cho phép nhập <br>- Hệ thống tự động strim sapce |
|  | 1. Thực hiện nhập data có chứa space ở cuối |  | 1. <br>- Hệ thống cho phép nhập <br>- Hệ thống tự động strim sapce |
|  | 1. Thực hiện nhập data có chứa space ở giữa các ký tự |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập dữ liệu < minlenght |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập dữ liệu = minlenght |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập minlength < data input < maxlength |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập dữ liệu = maxlenght |  | 1. Hệ thống cho phép nhập  |
|  | 1. Thực hiện nhập dữ liệu > maxlenght |  | 1. Hệ thống lấy tối đa x ký tự, từ ký tự thứ x+1 hệ thống chặn không cho nhập  |
| Kiểm tra khi  khi paste data | 1. Thực hiện paste chuỗi ký tự < minlenght |  | 1. Hệ thống cho phép paste chuỗi ký tự  |
|  | 1. Thực hiện paste chuỗi ký tự = minlenght |  | 1. Hệ thống cho phép paste chuỗi ký tự  |
|  | 1. Thực hiện paste chuỗi ký tự  |  | 1. Hệ thống cho phép paste chuỗi ký tự  |
|  | 1. Thực hiện paste chuỗi dữ liệu min< data< max |  | 1. Hệ thống cho phép paste chuỗi ký tự  |
|  | 1. Thực hiện paste chuỗi dữ liệu = max |  | 1. Hệ thống cho phép paste chuỗi ký tự  |
|  | 1. Thực hiện paste chuỗi dữ liệu > max |  | 1. Hệ thống lấy tối đa x ký tự, từ ký tự thứ x+1 hệ thống tự động loại bỏ các ký tự phía sau |
|  | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt |  | 1. Hệ thống nhận các toàn bộ các ký tự đã paste vào textbox  |
| Kiểm tra nhập dữ liệu khi textbox đang báo lỗi  | 1. Thực hiện nhập dữ liệu hợp lệ vào textbox  | 1. Textbox đang báo lỗi đỏ  | 1. Hệ thống tự động ẩn câu thông báo lỗi |
|  | 1. Thực hiện nhập dữ không liệu hợp lệ vào textbox  | 1. Textbox đang báo lỗi đỏ  | 1. Hệ thống báo msg lỗi tương đương  |
| Kiểm tra khi nhập dữ liệu sau đó xoá toàn bộ data đã nhập | 1. Thực hiện nhập dữ liệu cho textbox <br>2. Xóa toàn bộ dữ liệu đã nhập  |  | 2. <br>- Clear toàn bộ dữ liệu ở textbox <br>- Báo lỗi/Không báo lỗi  |


## Label 

| Label | Kiểm tra hiển thị label  |  |  | - Label hiển thị đúng text theo đặc tả (font, size, color, style) design <br>- Không bị tràn, cắt chữ, xuống dòng sai.<br>- Căn chỉnh đúng với control liên quan (textbox, checkbox, radio, dropdown). |
| --- | --- | --- | --- | --- |
|  | Kiểm tra nội dung trên label  |  |  | Hiển thị chính xác text, không lỗi  |
|  | Kiểm tra khi thay đổi ngôn ngữ <br>(multi-language) |  |  | - Không bị vỡ label<br>- Ngôn ngữ được thay đổi theo đúng language  |
|  | Kiểm tra khi di chuyển chuột vào label  |  |  | - Hiển thị con trỏ chuột <br>- Không thay đổi màu sắc của label  |
|  | Kiểm tra khi click vào label  |  |  | Không cho phép người dùng click vào label  |


## Button

| Button | Kiểm tra khi di chuyển con trỏ chuột vào button  |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Màu sắc của button có sự thay đổi  |
| --- | --- | --- | --- | --- |
|  | Kiểm tra trạng thái button  |  | 1. Kiểm tra trạng thái của button  | 1. Button ở trạng thái enable và cho phép người dùng click  |
|  | Kiểm tra kết quả sau khi nhấn vào button  |  | 1. Thực hiện nhấn chọn vào button | 1. Hệ thống hiển thị màn hình/popup/thực hiện thêm mới/chỉnh sửa/tìm kiếm/...... |
|  | Kiểm tra khi thực hiện double click  |  |  | 1. Hiển thị biểu tượng loading và không cho phép double click |


## LinkHyperlink

| Link/Hyperlink  | Kiểm tra style link |  |  | Link/Hyperlink hiển thị màu xanh, gạch chân  |
| --- | --- | --- | --- | --- |
|  | Kiểm tra khi di chuyển con trỏ chuột vào link/hyperlink  |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Link đổi màu hoặc underline theo thiết kế |
|  | Kiểm tra responsive |  |  | Link không bị che, hiển thị rõ |
|  | Kiểm tra visited link |  | Click vào link rồi quay lại | Link đổi màu theo CSS visited |
|  | Kiểm tra khi click vào link/hyperlink  | Link Internal | Click vào link nội bộ | Mở đúng trang trong hệ thống |
|  |  | Link External | Click vào link ngoài | Mở đúng website ngoài, thường trong tab mới |
|  |  | Anchor | Click vào link #section | Trang scroll đến đúng vị trí section |
|  |  | Email | Click vào link trong email | Dẫn đến đúng trang trong hệ thống |
|  | Kiểm tra link khi bị disable |  | Thực hiện click vào link đang ở trạng thái disable  | Không thể click, không redirect |


