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

## 4. QUY TẮC NGHIỆP VỤ & DÙNG CHUNG (COMMON RULES & BUSINESS LOGIC)

### 4.1 Quy Tắc Dùng Chung (Common Rules Ref)

| Mã Quy Tắc | Tên Quy Tắc | Chi Tiết Quy Tắc (Specification Detail) |
| :--- | :--- | :--- |
| **CMR03** | Label Display Spec | • **Mục tiêu:** Hiển thị văn bản tĩnh (tiêu đề, mô tả, gợi ý) trên UI không có tương tác.<br>• **Phạm vi:** Áp dụng toàn ứng dụng cho văn bản tĩnh (Form titles, hints, support notes...). Không dùng cho buttons/links.<br>• **Tính năng:** Hiển thị văn bản không tab, tự động cập nhật ngôn ngữ khi chuyển đổi ngôn ngữ, hỗ trợ chèn dữ liệu động (dynamic variables). |
| **CMR04** | Input Validation Rule Spec | • **Frontend UI:** Không thực hiện frontend validation kèm thông báo lỗi trực tiếp. Chặn hành vi không hợp lệ ngay tại UI (ví dụ: trường chỉ nhập số sẽ ngăn gõ chữ/ký tự đặc biệt).<br>• **Trường số (Numeric Input):** Chỉ cho phép 0-9. Ngăn chữ, ký tự đặc biệt, khoảng trắng ngay khi gõ.<br>• **Trường bắt buộc (Required):** Nếu để trống, nút bấm vẫn cho phép click, việc kiểm tra do Backend xử lý và trả về thông báo lỗi.<br>• **Độ dài (Max/Min Length):** Không kiểm tra real-time trên FE; kiểm tra khi Submit hoặc phía Server-side. |
| **CMR41** | Required Fields Validation | • **Backend Check:** Phía Backend xác thực tất cả các trường bắt buộc để đảm bảo không bị trống hoặc thiếu.<br>• Nếu thiếu/trống: Backend trả về mã lỗi `CMMSG29` chi tiết cho trường vi phạm.<br>• **Frontend Display:** Nhận phản hồi lỗi từ Backend và hiển thị thông báo lỗi ngay bên cạnh/dưới trường nhập liệu tương ứng. |

### 4.2 Luồng Xử Lý Chi Tiết Tính Năng Đăng Nhập (Login Processing Flow)

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

## 5. DANH MỤC THÔNG BÁO LỖI (ERROR MESSAGES MASTER)

| Error Code | Description | Title JP | Title EN | Japanese Message | English / Localized Message |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MSG10** | ERR_INVALID_CRED | ログインエラー | Login Error | メールアドレス・IDまたはパスワードが一致しません。もう一度入力してください。 | The email address/ID or password does not match. Please try again. (Email/ID hoặc mật khẩu không đúng. Vui lòng thử lại.) |
| **CMMSG29** | ERR_REQUIRED_FIELD | 入力 sai/未入力 | Required Field Missing | 必須項目が入力されていません。 | Required field is missing or empty. Please check the input values. |
| **CMMSG54** | ERR_INVALID_FORMAT | フォーマットエラー | Invalid Input Format | 入力形式が gh文字/ số 不正です。 | Invalid character or format entered. Please re-enter. |

---

> **Ghi Chú Dành Cho Đội Ngũ Phát Triển (Dev & QA Note):**
> - **Security:** Mật khẩu truyền đi trong request body bắt buộc phải được mã hóa qua SSL/TLS (HTTPS). Tránh ghi log mật khẩu dưới dạng plaintext.
> - **UX:** Nút đăng nhập không bị disable khi để trống trường dữ liệu, nhưng sẽ kích hoạt hiển thị lỗi bên dưới input sau khi nhận phản hồi thất bại từ Backend.