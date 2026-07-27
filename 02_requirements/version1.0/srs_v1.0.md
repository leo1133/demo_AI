# TÀI LIỆU ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS)

**Dự Án:** Hệ Thống Website Mua Sắm Trực Tuyến (E-Commerce Platform)

| | |
|---|---|
| **Tên dự án** | E-Commerce Web Application |
| **Vai trò người viết** | Business Analyst (BA) |
| **Phiên bản** | v1.0 |
| **Ngày khởi tạo** | 30/06/2026 |
| **Trạng thái** | Chờ Phê Duyệt (Draft) |

---

## 1. Tổng Quan Dự Án

### 1.1. Mục đích
Tài liệu này đặc tả chi tiết các yêu cầu chức năng và phi chức năng cho hệ thống Website Mua sắm Thương mại điện tử (B2C Model). Mục tiêu là cung cấp một tài liệu hướng dẫn chuẩn xác cho đội ngũ Phát triển (Developers), Kiểm thử (QA/QC), và các bên liên quan (Stakeholders) nắm rõ luồng nghiệp vụ và quy tắc hoạt động của hệ thống.

### 1.2. Phạm vi hệ thống
Hệ thống tập trung vào việc cung cấp trải nghiệm mua sắm mượt mà cho Khách hàng từ bước tiếp cận sản phẩm, quản lý giỏ hàng, thực hiện đặt hàng đến theo dõi lịch sử đơn hàng cá nhân. Phiên bản này tập trung vào 7 cấu phần lõi: Đăng ký, Đăng nhập, Quên mật khẩu, Trang chủ, Giỏ hàng, Thanh toán và Lịch sử mua hàng.

### 1.3. Đối tượng người dùng (Actors)
- **Khách vãng lai (Guest):** Người dùng chưa có tài khoản hoặc chưa đăng nhập hệ thống. Có quyền xem trang chủ, duyệt sản phẩm và thêm sản phẩm vào giỏ hàng tạm thời.
- **Khách hàng thành viên (Customer):** Người dùng đã đăng ký và đăng nhập thành công. Có đầy đủ quyền hạn mua sắm, thanh toán và xem lịch sử đơn hàng.

---

## 2. Yêu Cầu Chức Năng Chi Tiết (Functional Requirements)

### 2.1. UC01: Đăng ký tài khoản (Register)

| | |
|---|---|
| **Mục tiêu** | Cho phép khách vãng lai tạo tài khoản thành viên để mua sắm và lưu trữ thông tin đơn hàng. |
| **Điều kiện tiên quyết** | Khách hàng truy cập vào trang Đăng ký từ Link trên Header. |
| **Luồng xử lý chính (Main Flow)** | 1. Hệ thống hiển thị Form đăng ký gồm: Họ và tên, Email, Số điện thoại, Mật khẩu, Nhập lại mật khẩu.<br>2. Khách hàng nhập đầy đủ và chính xác thông tin.<br>3. Khách hàng nhấn nút "Đăng ký".<br>4. Hệ thống kiểm tra tính hợp lệ dữ liệu và tính duy nhất của Email/Số điện thoại.<br>5. Hệ thống tạo tài khoản mới, gửi mail kích hoạt và thông báo "Đăng ký thành công". |
| **Luồng ngoại lệ (Exceptions)** | - **Email/SĐT đã tồn tại:** Hệ thống báo lỗi "Email hoặc Số điện thoại đã được đăng ký".<br>- **Mật khẩu không khớp:** Hệ thống báo lỗi "Mật khẩu nhập lại không trùng khớp". |

**Business Rules (Quy tắc nghiệp vụ) - UC01:**
- Mật khẩu bắt buộc từ 8-20 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số và 1 ký tự đặc biệt.
- Email phải đúng định dạng chuẩn RFC 5322.

### 2.2. UC02: Đăng nhập (Login)

| | |
|---|---|
| **Mục tiêu** | Xác thực danh tính người dùng để cho phép truy cập vào các tính năng thành viên. |
| **Dữ liệu đầu vào** | Email/Số điện thoại và Mật khẩu. |
| **Luồng xử lý chính (Main Flow)** | 1. Người dùng nhập Email/SĐT và Mật khẩu trên giao diện Đăng nhập.<br>2. Người dùng nhấn nút "Đăng nhập".<br>3. Hệ thống kiểm tra thông tin trong Cơ sở dữ liệu.<br>4. Thông tin chính xác, hệ thống chuyển hướng người dùng về Trang chủ dưới trạng thái đã đăng nhập. |
| **Luồng ngoại lệ (Exceptions)** | - **Sai thông tin:** Hệ thống hiển thị thông báo "Email/Số điện thoại hoặc Mật khẩu không chính xác". Không chỉ rõ sai trường nào để bảo mật.<br>- **Tài khoản bị khóa:** Báo lỗi "Tài khoản của bạn đã bị khóa. Vui lòng liên hệ CSKH". |

### 2.3. UC03: Quên mật khẩu (Forgot Password)

| | |
|---|---|
| **Mục tiêu** | Cho phép người dùng cấp lại mật khẩu mới khi quên mật khẩu cũ qua xác thực Email. |
| **Luồng xử lý chính (Main Flow)** | 1. Người dùng click "Quên mật khẩu" tại màn hình Đăng nhập.<br>2. Hệ thống yêu cầu nhập Email đã đăng ký.<br>3. Người dùng nhập Email và bấm "Gửi yêu cầu".<br>4. Hệ thống xác thực Email tồn tại và gửi một mã OTP/Link đặt lại mật khẩu có thời hạn (Token) qua Email.<br>5. Người dùng click Link trong Email, hệ thống hiển thị màn hình "Đặt lại mật khẩu mới".<br>6. Người dùng nhập mật khẩu mới và bấm "Lưu". Hệ thống cập nhật mật khẩu thành công. |

### 2.4. UC04: Trang chủ (Home Page)

| | |
|---|---|
| **Mục tiêu** | Hiển thị giao diện chính, quảng bá sản phẩm, chương trình khuyến mãi và điều hướng người dùng. |
| **Các thành phần hiển thị chính** | - **Header:** Logo, Thanh tìm kiếm sản phẩm, Giỏ hàng (số lượng item), Nút Đăng nhập/Đăng ký (hoặc Tên người dùng nếu đã đăng nhập).<br>- **Banner Carousel:** Hiển thị các hình ảnh chiến dịch khuyến mãi lớn (tự động chuyển động).<br>- **Danh mục sản phẩm (Categories):** Các khối biểu tượng ngành hàng (Ví dụ: Thời trang, Điện tử, Gia dụng).<br>- **Khối sản phẩm (Product Grid):** Khối "Sản phẩm bán chạy" và "Sản phẩm mới nhất". Mỗi thẻ sản phẩm hiển thị: Hình ảnh, Tên sản phẩm, Giá gốc, Giá bán, Nút "Thêm vào giỏ".<br>- **Footer:** Thông tin liên hệ, chính sách mua hàng, liên kết mạng xã hội. |

### 2.5. UC05: Giỏ hàng (Shopping Cart)

| | |
|---|---|
| **Mục tiêu** | Lưu trữ và quản lý các sản phẩm khách hàng dự định mua trước khi tiến hành thanh toán. |
| **Tính năng chi tiết** | - **Thêm vào giỏ:** Khách chọn sản phẩm từ Trang chủ/Trang chi tiết, chọn số lượng và thêm vào giỏ.<br>- **Cập nhật số lượng:** Cho phép tăng/giảm số lượng trực tiếp trong trang giỏ hàng bằng nút (+ / -) hoặc nhập số.<br>- **Xóa sản phẩm:** Cho phép xóa từng sản phẩm hoặc xóa toàn bộ giỏ hàng.<br>- **Tính toán tự động:** Tự động tính Thành tiền cho từng dòng và Tổng tiền (Subtotal) cho toàn bộ giỏ hàng theo thời gian thực (Real-time). |

**Business Rules (Quy tắc nghiệp vụ) - UC05:**
- Số lượng một mặt hàng trong giỏ hàng không được vượt quá số lượng tồn kho khả dụng của sản phẩm đó.
- Khách vãng lai được lưu giỏ hàng trong Session/Cookie (tối đa 7 ngày). Khi khách đăng nhập, hệ thống phải tự động gộp (merge) giỏ hàng tạm thời vào giỏ hàng của tài khoản thành viên.

### 2.6. UC06: Thanh toán (Checkout)

| | |
|---|---|
| **Mục tiêu** | Xử lý thông tin giao hàng, áp dụng giảm giá và xác nhận đặt đơn hàng. |
| **Các bước thực hiện** | 1. Từ Giỏ hàng, người dùng bấm "Tiến hành thanh toán". Yêu cầu bắt buộc đăng nhập tại bước này.<br>2. Thông tin giao hàng: Người dùng nhập Họ tên, SĐT, Địa chỉ nhận hàng (Tỉnh/Thành phố, Quận/Huyện, Phường/Xã, Số nhà). Tự động điền nếu tài khoản đã có sẵn thông tin trước đó.<br>3. Phương thức vận chuyển: Hiển thị các đơn vị vận chuyển phối hợp kèm phí ship tương ứng.<br>4. Phương thức thanh toán: Chọn giữa 02 hình thức: COD (Thanh toán khi nhận hàng) hoặc Thanh toán trực tuyến (Thẻ nội địa, Ví điện tử).<br>5. Mã giảm giá (Coupon): Cho phép nhập mã giảm giá, hệ thống kiểm tra và trừ tiền trực tiếp vào tổng bill nếu hợp lệ.<br>6. Xác nhận đơn hàng: Bấm "Đặt hàng". Hệ thống trừ kho, tạo đơn hàng với trạng thái "Chờ xử lý" và gửi mail xác nhận đơn hàng cho khách. |

### 2.7. UC07: Lịch sử mua hàng (Purchase History)

| | |
|---|---|
| **Mục tiêu** | Giúp khách hàng theo dõi, quản lý trạng thái các đơn hàng đã và đang mua. |
| **Nội dung hiển thị** | - Hiển thị danh sách đơn hàng sắp xếp theo thời gian mới nhất lên đầu.<br>- Thông tin tổng quát mỗi đơn: Mã đơn hàng (Order ID), Ngày đặt, Tổng tiền, Trạng thái đơn hàng (Chờ xử lý, Đã xác nhận, Đang giao, Đã giao, Đã hủy).<br>- Xem chi tiết đơn hàng: Khi click vào mã đơn, hiển thị chi tiết danh sách sản phẩm đã mua, phí vận chuyển, số tiền được giảm, địa chỉ nhận hàng và lịch sử thay đổi trạng thái đơn. |

---

## 3. Yêu Cầu Phi Chức Năng (Non-functional Requirements)

### 3.1. Hiệu năng (Performance)
- Thời gian phản hồi (Response Time) của trang chủ và các trang danh mục sản phẩm phải dưới 2 giây dưới điều kiện mạng tiêu chuẩn.
- Hệ thống có khả năng chịu tải (Concurrency) tối thiểu 5,000 người dùng truy cập đồng thời tại một thời điểm mà không xảy ra lỗi sập hệ thống (Crash).

### 3.2. Bảo mật (Security)
- Tất cả dữ liệu truyền truyền qua internet phải sử dụng giao thức bảo mật HTTPS mã hóa SSL.
- Mật khẩu của người dùng phải được băm (hash) bằng các thuật toán bảo mật mạnh (ví dụ: bcrypt) trước khi lưu vào cơ sở dữ liệu.

### 3.3. Khả năng tương thích (Compatibility)
- Website phải hiển thị tốt và tối ưu hóa trải nghiệm trên cả thiết bị di động (Mobile Responsive), Máy tính bảng (Tablet) và Máy tính để bàn (Desktop).
- Hỗ trợ tốt trên các trình duyệt phổ biến hiện nay bao gồm: Google Chrome, Apple Safari, Mozilla Firefox và Microsoft Edge.
