# Tóm tắt Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) v1.1
**Dự án:** Hệ Thống Website Mua Sắm Trực Tuyến (E-Commerce Platform)

*(Phiên bản v1.1: Đã cập nhật và làm rõ các Business Rules, Validation, Exception dựa trên kết quả giải đáp Q&A trong buổi Requirement Review giữa BA và QA).*

### 1. Project Goal (Mục tiêu dự án)
* **Đối với người dùng cuối:** Cung cấp trải nghiệm mua sắm mượt mà cho Khách hàng từ bước tiếp cận sản phẩm, quản lý giỏ hàng, thực hiện đặt hàng đến theo dõi lịch sử đơn hàng cá nhân.
* **Đối với nội bộ:** Cung cấp một tài liệu hướng dẫn chuẩn xác cho đội ngũ Phát triển (Developers), Kiểm thử (QA/QC), và các bên liên quan (Stakeholders) nắm rõ luồng nghiệp vụ và quy tắc hoạt động của hệ thống.

### 2. Business Objective (Mục tiêu kinh doanh)
* **Chưa được đề cập.** (Tài liệu tập trung vào mục đích của hệ thống và tài liệu thay vì các chỉ số hay mục tiêu kinh doanh cụ thể).

### 3. User Roles (Đối tượng người dùng)
Hệ thống phân chia thành 2 nhóm đối tượng chính (Actors):
* **Khách vãng lai (Guest):** Người dùng chưa có tài khoản hoặc chưa đăng nhập. Có quyền xem trang chủ, duyệt sản phẩm và thêm sản phẩm vào giỏ hàng tạm thời.
* **Khách hàng thành viên (Customer):** Người dùng đã đăng ký và đăng nhập thành công. Có đầy đủ quyền hạn mua sắm, thanh toán và xem lịch sử đơn hàng.

### 4. Main Modules (Các module/cấu phần chính)
Phiên bản v1.1 tập trung vào 7 cấu phần lõi:
1. Đăng ký tài khoản (Register)
2. Đăng nhập (Login)
3. Quên mật khẩu (Forgot Password)
4. Trang chủ (Home Page)
5. Giỏ hàng (Shopping Cart)
6. Thanh toán (Checkout)
7. Lịch sử mua hàng (Purchase History)

### 5. Business Flow (Luồng nghiệp vụ)
Luồng mua sắm cơ bản được mô tả theo trình tự sau:
* **Tiếp cận:** Xem Trang chủ / duyệt danh mục sản phẩm $\rightarrow$ **Giỏ hàng:** Thêm sản phẩm vào giỏ, cập nhật số lượng $\rightarrow$ **Xác thực:** Đăng ký/Đăng nhập (bắt buộc trước khi thanh toán. *Đã làm rõ: Khi đăng nhập thành công tại bước này, hệ thống tự động redirect về lại trang Checkout*) $\rightarrow$ **Thanh toán:** Nhập thông tin giao hàng, chọn phương thức vận chuyển/thanh toán, áp dụng mã giảm giá và Xác nhận đặt hàng $\rightarrow$ **Hậu mãi:** Xem lịch sử và trạng thái đơn hàng.

### 6. Business Rules & Validation (Quy tắc & Ràng buộc nghiệp vụ)
* **Về Tài khoản (UC01, UC02, UC03):** 
  * "Họ và tên": Giới hạn 2-50 ký tự, không được chứa số và ký tự đặc biệt.
  * "Số điện thoại": Gồm 10 chữ số, bắt đầu bằng số 0 (chuẩn Việt Nam).
  * Email không hợp lệ sẽ báo lỗi: "Email không đúng định dạng, vui lòng kiểm tra lại".
  * Mật khẩu: Bắt buộc từ 8-20 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số và 1 ký tự đặc biệt. Khi đặt lại mật khẩu mới (UC03), không được phép trùng với mật khẩu cũ (hiện tại).
  * Lock account: Tài khoản bị khóa trong 30 phút nếu nhập sai mật khẩu 5 lần liên tiếp.
  * Token/Link: Link kích hoạt tài khoản (UC01) có hiệu lực 24 giờ. Mã OTP/Link cấp lại mật khẩu (UC03) có hiệu lực 15 phút. Người dùng có thể nhấn nút "Gửi lại" nếu quá hạn.

* **Về Trang chủ (UC04):**
  * Sản phẩm bán chạy: Tính theo top doanh số trong 30 ngày gần nhất. 
  * Sản phẩm mới nhất: Các sản phẩm được đăng lên hệ thống trong vòng 7 ngày qua.
  * Banner Carousel: Tự động chuyển slide mỗi 5 giây.

* **Về Giỏ hàng (UC05):**
  * Nhập liệu: Không được nhập số 0, số âm, hoặc ký tự chữ/đặc biệt. Nếu nhập sai, hiển thị lỗi "Số lượng không hợp lệ". Số lượng không được vượt quá tồn kho.
  * Giới hạn: Một giỏ hàng chứa tối đa 50 loại sản phẩm (SKU) khác nhau.
  * Xử lý hết hàng: Nếu sản phẩm đang ở trong giỏ nhưng kho thực tế vừa hết, hệ thống vô hiệu hóa (disable) sản phẩm đó trong giỏ hàng, hiển thị dòng cảnh báo "Sản phẩm đã hết hàng" màu đỏ và không tính vào tổng tiền để Checkout.
  * Gộp giỏ hàng: Giỏ hàng khách vãng lai lưu trong Session/Cookie tối đa 7 ngày, và phải gộp (merge) tự động khi đăng nhập.

* **Về Thanh toán (UC06):**
  * Địa chỉ: Tỉnh/Thành phố, Quận/Huyện, Phường/Xã bắt buộc chọn từ Dropdown list.
  * Mã giảm giá (Coupon): Sẽ được kiểm tra điều kiện áp dụng (Ví dụ: giá trị đơn hàng tối thiểu). Nếu sai hiển thị lỗi: "Mã giảm giá không hợp lệ, không đủ điều kiện hoặc đã hết hạn".

* **Về Lịch sử mua hàng (UC07):**
  * Hủy đơn hàng: Khách hàng thành viên chỉ được phép nhấn "Hủy đơn hàng" khi đơn hàng đó đang ở trạng thái "Chờ xử lý".

### 7. Integration (Tích hợp)
* Tích hợp API đơn vị vận chuyển (Ví dụ: Giao Hàng Nhanh / Viettel Post) để nạp dữ liệu địa lý (Tỉnh/Thành/Quận/Huyện/Phường/Xã) và tính phí vận chuyển real-time.
* Xử lý ngoại lệ (Exception): Nếu API vận chuyển gặp sự cố/Timeout, hệ thống hiển thị thông báo "Không thể tính phí vận chuyển lúc này, vui lòng thử lại sau" và tạm thời vô hiệu hóa nút "Đặt hàng".

### 8. Non-functional Requirement (Yêu cầu phi chức năng)
* **Hiệu năng (Performance):** 
  * Thời gian phản hồi trang chủ và danh mục < 2 giây.
  * Chịu tải tối thiểu 5,000 người dùng truy cập đồng thời mà không bị lỗi.
* **Bảo mật (Security):** 
  * HTTPS mã hóa SSL toàn hệ thống.
  * Mật khẩu phải được băm bằng bcrypt.
* **Tương thích (Compatibility):** 
  * Responsive đa thiết bị (Mobile, Tablet, Desktop).
  * Hỗ trợ trình duyệt phổ biến từ các phiên bản: Google Chrome (>= 90), Apple Safari (>= 14), Mozilla Firefox (>= 90), Microsoft Edge (>= 90).
