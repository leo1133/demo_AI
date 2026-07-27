# Bối cảnh dự án cho AI & người mới

# Tóm tắt Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) v1.0
**Dự án:** Hệ Thống Website Mua Sắm Trực Tuyến (E-Commerce Platform)

Dưới góc độ của một Senior Business Analyst và Senior QA, các thông tin dưới đây được trích xuất hoàn toàn từ tài liệu SRS, tuân thủ nghiêm ngặt ràng buộc không suy diễn:

### 1. Project Goal (Mục tiêu dự án)
* **Đối với người dùng cuối:** Cung cấp trải nghiệm mua sắm mượt mà cho Khách hàng từ bước tiếp cận sản phẩm, quản lý giỏ hàng, thực hiện đặt hàng đến theo dõi lịch sử đơn hàng cá nhân.
* **Đối với nội bộ:** Cung cấp một tài liệu hướng dẫn chuẩn xác cho đội ngũ Phát triển (Developers), Kiểm thử (QA/QC), và các bên liên quan (Stakeholders) nắm rõ luồng nghiệp vụ và quy tắc hoạt động của hệ thống.

### 2. User Roles (Đối tượng người dùng)
Hệ thống phân chia thành 2 nhóm đối tượng chính (Actors):
* **Khách vãng lai (Guest):** Người dùng chưa có tài khoản hoặc chưa đăng nhập. Có quyền xem trang chủ, duyệt sản phẩm và thêm sản phẩm vào giỏ hàng tạm thời.
* **Khách hàng thành viên (Customer):** Người dùng đã đăng ký và đăng nhập thành công. Có đầy đủ quyền hạn mua sắm, thanh toán và xem lịch sử đơn hàng.

### 3. Main Modules (Các module/cấu phần chính)
Phiên bản v1.0 tập trung vào 7 cấu phần lõi (tương ứng với 7 Use Case chi tiết):
1. Đăng ký tài khoản (Register)
2. Đăng nhập (Login)
3. Quên mật khẩu (Forgot Password)
4. Trang chủ (Home Page)
5. Giỏ hàng (Shopping Cart)
6. Thanh toán (Checkout)
7. Lịch sử mua hàng (Purchase History)

### 4. Business Flow (Luồng nghiệp vụ)
Dựa vào phạm vi hệ thống (mục 1.2) và các Use Case (từ 2.1 đến 2.7), luồng mua sắm cơ bản được mô tả theo trình tự sau:
* **Tiếp cận:** Xem Trang chủ / duyệt danh mục sản phẩm $\rightarrow$ **Giỏ hàng:** Thêm sản phẩm vào giỏ, cập nhật số lượng $\rightarrow$ **Xác thực:** Đăng ký/Đăng nhập (bắt buộc trước khi thanh toán) $\rightarrow$ **Thanh toán:** Nhập thông tin giao hàng, chọn phương thức vận chuyển/thanh toán, áp dụng mã giảm giá và Xác nhận đặt hàng $\rightarrow$ **Hậu mãi:** Xem lịch sử và trạng thái đơn hàng.

### 5. Business Rules (Quy tắc nghiệp vụ)
Các quy tắc nghiệp vụ cốt lõi được ghi nhận:
* **Về Tài khoản (UC01):** 
  * Mật khẩu bắt buộc từ 8-20 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số và 1 ký tự đặc biệt.
  * Email phải đúng định dạng chuẩn RFC 5322.
* **Về Giỏ hàng (UC05):**
  * Số lượng một mặt hàng trong giỏ không được vượt quá số lượng tồn kho khả dụng.
  * Giỏ hàng của Khách vãng lai được lưu trong Session/Cookie tối đa 7 ngày.
  * Hệ thống phải tự động gộp (merge) giỏ hàng tạm thời vào giỏ hàng của tài khoản thành viên ngay khi khách đăng nhập.

### 6. Integration (Tích hợp)
* **Chưa được đề cập.** (Mặc dù phần Thanh toán có nhắc đến "Thanh toán trực tuyến" và "Đơn vị vận chuyển phối hợp", nhưng tài liệu không đặc tả chi tiết về việc tích hợp với hệ thống hay API của bên thứ 3 nào).

### 7. Non-functional Requirement (Yêu cầu phi chức năng)
Tài liệu định nghĩa 3 nhóm yêu cầu phi chức năng:
* **Hiệu năng (Performance):** 
  * Thời gian phản hồi (Response Time) của trang chủ và trang danh mục sản phẩm < 2 giây (mạng tiêu chuẩn).
  * Chịu tải (Concurrency) tối thiểu 5,000 người dùng truy cập đồng thời mà không bị lỗi sập hệ thống (Crash).
* **Bảo mật (Security):** 
  * Dữ liệu truyền tải qua Internet phải dùng HTTPS mã hóa SSL.
  * Mật khẩu người dùng phải được băm (hash) bằng thuật toán mạnh (VD: bcrypt) trước khi lưu vào Cơ sở dữ liệu.
* **Tương thích (Compatibility):** 
  * Hiển thị tốt/tối ưu trên đa thiết bị: Di động (Mobile Responsive), Máy tính bảng (Tablet), Máy tính để bàn (Desktop).
  * Hỗ trợ các trình duyệt phổ biến: Google Chrome, Apple Safari, Mozilla Firefox, Microsoft Edge.
