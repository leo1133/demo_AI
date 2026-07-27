# Báo cáo Requirement Review (Góc độ QA)
**Dự án:** Hệ Thống Website Mua Sắm Trực Tuyến (E-Commerce Platform)
**Vai trò:** Senior QA

Dưới đây là danh sách các câu hỏi cần làm rõ với Business Analyst (BA) trong buổi Requirement Review, được phân tích dựa trên tài liệu SRS v1.0. Các câu hỏi nhằm phát hiện những lỗ hổng, sự mơ hồ và thiếu sót trong logic hệ thống trước khi tiến hành viết Test Case.

## 1. UC01: Đăng ký tài khoản (Register)
- **[High] Validation chưa rõ:** Trường "Họ và tên" có giới hạn độ dài tối thiểu/tối đa (Min/Max length) và ký tự cho phép (có được chứa số, ký tự đặc biệt không) như thế nào?
- **[High] Validation chưa rõ:** Định dạng hợp lệ của "Số điện thoại" là gì? (Bắt buộc bao nhiêu chữ số, có cần mã vùng quốc gia hay bắt buộc theo các đầu số nhà mạng cụ thể không?)
- **[Medium] Error Message chưa rõ:** Nếu người dùng nhập Email không đúng định dạng chuẩn RFC 5322, hệ thống sẽ hiển thị câu thông báo lỗi chính xác là gì?
- **[High] Business Rule/Exception chưa rõ:** Khi hệ thống "gửi mail kích hoạt", Link/Token kích hoạt này có thời hạn sống (Expiration time) là bao lâu? Nếu quá hạn kích hoạt thì quy trình xử lý tiếp theo của người dùng là gì (có nút gửi lại mail không)?

## 2. UC02: Đăng nhập (Login)
- **[High] Business Rule chưa rõ:** Điều kiện cụ thể nào dẫn đến việc "Tài khoản bị khóa"? (Ví dụ: Đăng nhập sai mật khẩu bao nhiêu lần liên tiếp?). Và tài khoản bị khóa trong khoảng thời gian bao lâu, hay khóa vĩnh viễn cho đến khi liên hệ CSKH?

## 3. UC03: Quên mật khẩu (Forgot Password)
- **[High] Boundary chưa rõ:** Mã OTP/Link đặt lại mật khẩu "có thời hạn" cụ thể là bao nhiêu phút/giờ?
- **[High] Validation chưa rõ:** Khi người dùng nhập "mật khẩu mới", mật khẩu này có bắt buộc phải tuân theo Business Rule của UC01 (8-20 ký tự, có hoa, thường, số, đặc biệt) hay không? 
- **[Medium] Exception chưa rõ:** Khi đặt mật khẩu mới, người dùng có được phép nhập lại mật khẩu cũ (mật khẩu hiện tại vừa quên) hay không? 

## 4. UC04: Trang chủ (Home Page)
- **[Medium] Business Rule chưa rõ:** Tiêu chí/Logic nào để hệ thống xác định một sản phẩm thuộc khối "Sản phẩm bán chạy" (Top doanh số theo tuần/tháng?) và "Sản phẩm mới nhất" (Đăng lên hệ thống trong vòng X ngày?)
- **[Low] Boundary chưa rõ:** Banner Carousel tự động chuyển động với tần suất (thời gian delay giữa các ảnh) là bao nhiêu giây? 

## 5. UC05: Giỏ hàng (Shopping Cart)
- **[High] Validation/Boundary chưa rõ:** Tại tính năng "Cập nhật số lượng" (nhập số), người dùng có bị chặn nhập số 0, số âm, hoặc ký tự chữ/đặc biệt không? Error message là gì nếu nhập sai định dạng?
- **[Low] Boundary chưa rõ:** Một giỏ hàng có giới hạn số lượng tối đa các loại sản phẩm (SKU) khác nhau được thêm vào không?
- **[High] Exception chưa rõ:** Trong trường hợp khách hàng đang lưu sản phẩm A trong giỏ, nhưng số lượng tồn kho thực tế của sản phẩm A vừa hết (do người khác thanh toán thành công), hệ thống sẽ xử lý cảnh báo (Error message/UI update) như thế nào đối với khách hàng đang ở trang giỏ hàng?

## 6. UC06: Thanh toán (Checkout)
- **[Medium] Business Flow chưa rõ:** Tại bước "Yêu cầu bắt buộc đăng nhập tại bước này", sau khi khách vãng lai đăng nhập thành công, hệ thống có tự động điều hướng (redirect) trở lại chính xác trang Checkout để tiếp tục thanh toán không, hay bị đưa về Trang chủ?
- **[High] Validation/Integration chưa rõ:** Thông tin giao hàng (Tỉnh/Thành phố, Quận/Huyện, Phường/Xã) là các trường nhập liệu tự do (Text box) hay danh sách chọn (Dropdown list) được đồng bộ từ API của bên vận chuyển?
- **[High] Exception/Integration chưa rõ:** Nếu API của "các đơn vị vận chuyển phối hợp" gặp sự cố (Timeout/Error) dẫn đến không thể trả về phí ship, hệ thống sẽ xử lý và hiển thị thông báo gì cho người dùng? Khách hàng có được thanh toán tiếp không?
- **[Medium] Validation chưa rõ:** Mã giảm giá (Coupon) có các quy tắc áp dụng đi kèm không (Ví dụ: Giá trị đơn hàng tối thiểu)? Thông báo lỗi là gì khi nhập mã hết hạn, không tồn tại hoặc không đủ điều kiện?

## 7. UC07: Lịch sử mua hàng (Purchase History)
- **[High] Permission/Business Flow chưa rõ:** Khách hàng thành viên có quyền "Hủy đơn hàng" trực tiếp từ giao diện lịch sử mua hàng này không? Nếu có, họ chỉ được phép hủy khi đơn hàng đang ở trạng thái nào (Ví dụ: chỉ được hủy khi "Chờ xử lý")?

## 8. Yêu cầu Phi chức năng (Non-functional)
- **[Low] Boundary/Compatibility chưa rõ:** Hỗ trợ tốt trên trình duyệt phổ biến (Chrome, Safari, Firefox, Edge) nhưng quy định bắt buộc hỗ trợ từ phiên bản (Version) nào trở lên? (Để giới hạn phạm vi môi trường kiểm thử).
