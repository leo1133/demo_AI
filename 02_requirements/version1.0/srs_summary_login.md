# TÀI LIỆU YÊU CẦU PHẦN MỀM (SRS)
## Tính năng: Đăng nhập Hệ thống (Login - AA000_Login)

---

### Thông tin chung (Document Metadata)

| Thuộc tính | Giá trị |
| :--- | :--- |
| **Mã màn hình (Screen ID)** | `AA000_Login` |
| **Tên màn hình (Screen Name)** | Màn hình Đăng nhập (Login Screen) |
| **URL** | `https://admin-dev.surrealdolls.com/sign-in` |
| **Basic Auth** | `username`: `suriaru`/ `password`: `suriaru` |
| **Tài khoản demo** | `admin@admin.com` / `!Ch4ng3Th1sP4ssW0rd!` |
| **API Endpoint** | `POST /admin/auth/login/` |
| **Người tạo (Created By)** | BA |
| **Ngày tạo (Created Date)** | 2025/05/20 |
| **Cập nhật cuối (Updated Date)** | 2026/08/07 |

---

## 1. TỔNG QUAN TÍNH NĂNG (OVERVIEW)

Màn hình **AA000_Login** cho phép Quản trị viên / Người dùng (Admin / User) xác thực tài khoản vào hệ thống bằng Email / Login ID và Mật khẩu. Khi xác thực thành công, hệ thống cấp phát chuỗi xác thực (`access_token` & `refresh_token`) và điều hướng người dùng đến Trang chủ (Home page). Mọi hành vi đăng nhập đều được ghi nhận vào nhật ký truy cập (Access Log).

---

## 2. DANH SÁCH THÀNH PHẦN MÀN HÌNH (SCREEN ITEM DEFINITIONS)

| No | Tên Nhãn (JP) | Gợi Ý (Placeholder) | Loại (Type) | Bắt Buộc | Mô Tả & Quy Tắc Nghiệp Vụ | API Mapping |
| :---: | :--- | :--- | :---: | :---: | :--- | :--- |
| **1** | ログイン | - | label | - | Tham chiếu `CMR03` (Hiển thị tiêu đề màn hình). | - |
| **2** | メール/ID | - | label | - | Tham chiếu `CMR03` (Nhãn trường đăng nhập). | - |
| **3** | - | メールアドレス・ID入力 | input (textbox) | **Yes** | • Nhập Email hoặc Login ID.<br>• Quy tắc dùng chung: Tham chiếu `CMR04`.<br>• Kiểm tra bắt buộc: Tham chiếu `CMR41`. | `Request: email_user_id` |
| **4** | パスワード | - | label | - | Tham chiếu `CMR03` (Nhãn trường Mật khẩu). | - |
| **5** | - | パスワード入力 | input (textbox) | **Yes** | • Nhập Mật khẩu tài khoản.<br>• Hiển thị dạng ký tự ẩn (`********` mặc định).<br>• Quy tắc dùng chung: Tham chiếu `CMR04`, `CMR41`. | `Request: password` |
| **6** | パスワードを忘れた方 | - | textbutton | - | Nút liên kết chuyển hướng sang màn hình Quên mật khẩu (`AB001`). | - |
| **7** | ログイン | - | button | - | • Nút thực thi Đăng nhập.<br>• Hệ thống kiểm tra thông tin và cấp quyền truy cập.<br>• Trả về Token xác thực khi thành công. | `Response: access_token`<br>`Response: refresh_token` |

---

## 3. SỰ KIỆN MÀN HÌNH (LIST SCREEN EVENTS)

| No | Tên Sự Kiện (Screen Event) | Thời Điểm Kích Hoạt (Trigger Timing) | Màn Hình Chuyển Đến | Ghi Chú / API Liên Quan |
| :---: | :--- | :--- | :--- | :--- |
| **1** | Initial Display (Khởi tạo) | Khi màn hình Login được load hoặc hiển thị | - | Load giao diện chuẩn theo cấu hình ngôn ngữ hệ thống. |
| **2** | Go to Forgot Password | Người dùng bấm vào textbutton `パスワードを忘れた方` | `AB001` (Forgot Password) | Chuyển hướng trực tiếp tới màn hình khôi phục mật khẩu. |
| **3** | Login (Thực hiện Đăng nhập) | Người dùng bấm nút `ログイン` | Home page (Trang chủ) | Gọi API `POST /admin/auth/login/`. Nếu hợp lệ, chuyển về Home Page. |

---

## 4. Luồng Xử Lý Chi Tiết Tính Năng Đăng Nhập (Login Processing Flow)

Khi người dùng nhập `email_user_id` và `password`, sau đó ấn nút **ログイン**:

1. **Xác thực phía Backend:**
   - Kiểm tra các trường bắt buộc theo quy tắc `CMR41`. Nếu trống/thiếu, trả về mã lỗi `CMMSG29`.
   - Xác thực thông tin tài khoản: Kiểm tra `email_user_id` và `password` có chính xác trong CSDL hay không.

2. **Trường hợp Thất bại (Invalid):**
   - Nếu mật khẩu không đúng hoặc không tìm thấy tài khoản: Trả về mã lỗi `MSG10`.
   - **Ghi Log (Log access monitoring):** Ghi nhận nhật ký đăng nhập thất bại (`Login status: failure`) gồm: User ID, IP address, Login timestamp, Device/User-agent info (nếu có).

3. **Trường hợp Thành công (Success):**
   - Trả về mã xác thực: `access_token` và `refresh_token`.
   - **Ghi Log (Log access monitoring):** Ghi nhận nhật ký đăng nhập thành công (`Login status: success`) gồm đầy đủ thông tin: User ID, IP address, Login timestamp, Device/User-agent info.
   - Chuyển hướng người dùng sang Trang chủ (Home Page).

---

> **Ghi Chú Dành Cho Đội Ngũ Phát Triển (Dev & QA Note):**
> - **Security:** Mật khẩu truyền đi trong request body bắt buộc phải được mã hóa qua SSL/TLS (HTTPS). Tránh ghi log mật khẩu dưới dạng plaintext.
> - **UX:** Nút đăng nhập không bị disable khi để trống trường dữ liệu, nhưng sẽ kích hoạt hiển thị lỗi bên dưới input sau khi nhận phản hồi thất bại từ Backend.