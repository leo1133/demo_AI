# TEST CASE — Màn hình AA000_Login (Đăng nhập Hệ thống)

**Nguồn tham chiếu:** `srs_summary_login.md`, `Common_Spec.md` (CMR03, CMR04, CMR41), `Common_MSG.md` (CMMSG29), `MSG.md` (MSG10)
**API:** `POST /admin/auth/login/` | **Request:** `email_user_id`, `password` | **Response:** `access_token`, `refresh_token`
**Tài khoản demo:** `admin@admin.com` / `!Ch4ng3Th1sP4ssW0rd!`

---

## A. Nhóm: Hiển thị màn hình ban đầu (Initial Display)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_001 | Đã Basic Auth (`suriaru`/`suriaru`) vào được domain dev | N/A | 1. Truy cập `https://admin-dev.surrealdolls.com/sign-in` | Màn hình Login hiển thị đầy đủ: tiêu đề "ログイン", label "メール/ID", input Email/ID, label "パスワード", input Password, link "パスワードを忘れた方", button "ログイン" | High |
| TC_LOGIN_002 | Đã ở màn hình Login | N/A | 1. Quan sát input Email/ID khi chưa nhập gì | Placeholder "メールアドレス・ID入力"/hiển thị đúng theo placeholder quy định, không có ký tự nào bị lộ | Medium |
| TC_LOGIN_003 | Đã ở màn hình Login | N/A | 1. Quan sát input Password khi chưa nhập gì | Placeholder "パスワード入力"/hiển thị đúng theo placeholder quy định, không có ký tự nào bị lộ | Medium |
| TC_LOGIN_004 | Đã ở màn hình Login, hệ thống hỗ trợ đa ngôn ngữ (CMR05) | Ngôn ngữ hệ thống = English/Japanese | 1. Đổi ngôn ngữ hệ thống (nếu có switcher)<br>2. Reload màn hình Login | Toàn bộ label/placeholder/button chuyển theo đúng ngôn ngữ đã chọn (theo CMR03, CMR05) | Low |
| TC_LOGIN_005 | Đã ở màn hình Login | N/A | 1. Kiểm tra trạng thái mặc định của button "ログイン" khi 2 field còn trống | Button vẫn ở trạng thái **enabled** (không bị disable) — đúng theo Dev/QA Note & CMR04 | High |

---

## B. Nhóm: Input Field — Email/Login ID (Equivalence Partitioning + Error Guessing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_006 | Ở màn hình Login | `email_user_id = admin@admin.com` (hợp lệ, dạng email) | 1. Nhập vào field Email/ID | Text được nhập bình thường, hiển thị rõ ràng (không bị ẩn) | Medium |
| TC_LOGIN_007 | Ở màn hình Login | `email_user_id = admin01` (dạng Login ID, không phải email) | 1. Nhập vào field Email/ID | Field chấp nhận input dạng Login ID (field hỗ trợ cả 2 dạng theo SRS mục 2, No.3) | Medium |
| TC_LOGIN_008 | Ở màn hình Login | `email_user_id = "  admin@admin.com  "` (có khoảng trắng đầu/cuối) | 1. Nhập giá trị có leading/trailing space<br>2. Nhập password đúng<br>3. Bấm "ログイン" | Backend xử lý đúng (trim) và login thành công, HOẶC nếu không trim thì trả lỗi MSG10 nhất quán — không được crash / lỗi 500 | Medium |
| TC_LOGIN_009 | Ở màn hình Login | `email_user_id = ""` (rỗng) | 1. Để trống field Email/ID<br>2. Nhập password bất kỳ<br>3. Bấm "ログイン" | FE **không** hiện lỗi ngay khi rời field (theo CMR04); sau khi bấm Login, Backend trả về CMMSG29 dạng "メール/IDは空欄にできません。" và lỗi hiển thị sát field tương ứng | High |
| TC_LOGIN_010 | Ở màn hình Login | `email_user_id` = chuỗi rất dài (500+ ký tự) | 1. Nhập chuỗi cực dài vào field Email/ID<br>2. Nhập password hợp lệ<br>3. Bấm "ログイン" | Không có real-time validate FE (đúng CMR04); Backend xử lý (reject hợp lý hoặc trả lỗi rõ ràng), không gây lỗi 500/crash | Low |
| TC_LOGIN_011 | Ở màn hình Login | `email_user_id` chứa ký tự đặc biệt/emoji/SQL injection payload (`' OR '1'='1`, `<script>alert(1)</script>`) | 1. Nhập input như trên<br>2. Bấm "ログイン" | Hệ thống không bị SQL injection/XSS; trả về MSG10 (invalid credentials) hoặc lỗi hợp lệ khác, không lộ thông tin hệ thống | High |
| TC_LOGIN_012 | Ở màn hình Login | `email_user_id = "Admin@Admin.com"` (khác hoa/thường so với tài khoản đã đăng ký `admin@admin.com`) | 1. Nhập email khác hoa/thường<br>2. Nhập đúng password<br>3. Bấm "ログイン" | Xác nhận rõ hành vi: login thành công (case-insensitive) hoặc thất bại với MSG10 (case-sensitive) — ghi nhận kết quả thực tế vì SRS chưa quy định rõ | Medium |

---

## C. Nhóm: Input Field — Password (Equivalence Partitioning + UI State)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_013 | Ở màn hình Login | `password = !Ch4ng3Th1sP4ssW0rd!` | 1. Nhập password | Ký tự nhập vào hiển thị dạng ẩn (`•` hoặc `*`), không hiển thị plaintext trên UI | High |
| TC_LOGIN_014 | Ở màn hình Login | `password` bất kỳ | 1. Kiểm tra network request (DevTools) khi submit | Password được truyền qua HTTPS (SSL/TLS), không xuất hiện dưới dạng plaintext trong URL/query string | High |
| TC_LOGIN_015 | Có quyền truy cập log hệ thống (env test) | `password` bất kỳ, login thất bại | 1. Login sai vài lần<br>2. Kiểm tra access log/server log | Log **không** chứa password dưới dạng plaintext (theo Dev/QA Note) | High |
| TC_LOGIN_016 | Ở màn hình Login | `password = ""` (rỗng) | 1. Nhập email hợp lệ<br>2. Để trống Password<br>3. Bấm "ログイン" | Backend trả CMMSG29 "パスワードは空欄にできません。", lỗi hiển thị sát field Password | High |

---

## D. Nhóm: Required Field Validation — Decision Table (CMR41 → CMMSG29)

| Test Case ID | Preconditions | Test Data (Email / Password) | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_017 | Ở màn hình Login | Email: rỗng / Password: rỗng | 1. Để trống cả 2 field<br>2. Bấm "ログイン" | Backend trả CMMSG29 cho cả 2 field (hoặc field đầu tiên theo thứ tự ưu tiên BE), cả 2 lỗi hiển thị đúng vị trí | High |
| TC_LOGIN_018 | Ở màn hình Login | Email: có giá trị / Password: rỗng | 1. Nhập Email hợp lệ<br>2. Để trống Password<br>3. Bấm "ログイン" | Chỉ CMMSG29 cho Password hiển thị | High |
| TC_LOGIN_019 | Ở màn hình Login | Email: rỗng / Password: có giá trị | 1. Để trống Email<br>2. Nhập Password bất kỳ<br>3. Bấm "ログイン" | Chỉ CMMSG29 cho Email hiển thị | High |
| TC_LOGIN_020 | Ở màn hình Login | Email: có giá trị (sai) / Password: có giá trị (sai) | 1. Nhập cả 2 field với giá trị sai<br>2. Bấm "ログイン" | Không có lỗi "empty field"; chuyển sang luồng xác thực → nhận MSG10 | High |

---

## E. Nhóm: Business Rule — Xác thực đăng nhập (Decision Table)

| Test Case ID | Preconditions | Test Data (Email đúng/sai × Password đúng/sai) | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_021 | Tài khoản demo tồn tại: `admin@admin.com` | Email: đúng / Password: đúng | 1. Nhập `admin@admin.com` / `!Ch4ng3Th1sP4ssW0rd!`<br>2. Bấm "ログイン" | Login thành công → nhận `access_token`, `refresh_token` → điều hướng Home page | High |
| TC_LOGIN_022 | Tài khoản demo tồn tại | Email: đúng / Password: sai (`WrongPass123!`) | 1. Nhập đúng email, sai password<br>2. Bấm "ログイン" | Login thất bại → hiển thị MSG10 ("メールアドレス・IDまたはパスワードが一致しません。もう一度入力してください。") | High |
| TC_LOGIN_023 | N/A | Email: không tồn tại (`notexist@abc.com`) / Password: bất kỳ | 1. Nhập email không tồn tại trong hệ thống<br>2. Nhập password bất kỳ<br>3. Bấm "ログイン" | Login thất bại → MSG10 (thông báo giống hệt case sai password, **không tiết lộ** "tài khoản không tồn tại" — tránh lộ thông tin user enumeration) | High |
| TC_LOGIN_024 | Tài khoản demo tồn tại | Email: đúng (dạng Login ID nếu có) / Password: đúng | 1. Đăng nhập bằng Login ID (nếu tài khoản có Login ID riêng thay vì email)<br>2. Bấm "ログイン" | Login thành công tương tự như dùng email | Medium |
| TC_LOGIN_025 | Tài khoản demo tồn tại | Email: đúng / Password: đúng nhưng sai khác 1 ký tự cuối | 1. Nhập password gần đúng (thiếu/dư 1 ký tự)<br>2. Bấm "ログイン" | MSG10 hiển thị, không login được (kiểm tra password check là exact-match, không có fuzzy match) | Medium |
| TC_LOGIN_026 | Tài khoản demo tồn tại | Email/Password đúng nhưng có thêm khoảng trắng trong password | 1. Nhập `!Ch4ng3Th1sP4ssW0rd! ` (dư space cuối)<br>2. Bấm "ログイン" | Login thất bại với MSG10 (password không được tự động trim, vì trim password có thể gây lỗ hổng bảo mật) | Medium |

---

## F. Nhóm: Workflow — Điều hướng (Use Case Testing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_027 | Ở màn hình Login | N/A | 1. Bấm link "パスワードを忘れた方" | Hệ thống điều hướng đúng sang màn hình `AB001` (Forgot Password) | High |
| TC_LOGIN_028 | Ở màn hình Login, đã nhập dở email/password | N/A | 1. Nhập dở dữ liệu vào 2 field<br>2. Bấm "パスワードを忘れた方"<br>3. Bấm Back quay lại Login | Xác nhận hành vi: dữ liệu đã nhập có bị mất hay không (ghi nhận thực tế, vì SRS không quy định rõ) | Low |
| TC_LOGIN_029 | Login thành công | N/A | 1. Thực hiện TC_LOGIN_020<br>2. Quan sát sau khi login | Điều hướng đúng tới Home page, không còn ở lại màn hình Login, không cho back lại Login bằng nút Back trình duyệt mà mất phiên | Medium |

---

## G. Nhóm: State Transition — Trạng thái nút Login & hiển thị lỗi

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_030 | Ở màn hình Login (state: Initial/Idle) | Cả 2 field rỗng | 1. Bấm "ログイン" ngay khi field trống (không disable)<br>2. Quan sát UI trong lúc chờ response | Button chuyển sang state "Submitting" (loading nếu có), sau khi BE trả lỗi → chuyển sang state "Error displayed" với CMMSG29 sát field | High |
| TC_LOGIN_031 | State: "Error displayed" (từ TC_LOGIN_030) | Nhập lại dữ liệu hợp lệ | 1. Từ state lỗi, nhập giá trị hợp lệ vào field<br>2. Bấm "ログイン" lại | Lỗi cũ được xoá/cập nhật đúng, chuyển sang state "Authenticated" nếu dữ liệu đúng | Medium |
| TC_LOGIN_032 | Ở màn hình Login | Click "ログイン" liên tục nhiều lần khi request đang pending (double click / double submit) | 1. Bấm "ログイン" 2 lần liên tiếp thật nhanh | Không gửi 2 request trùng lặp gây lỗi logic (session/log bị nhân đôi); button nên tạm khoá trong lúc pending | Medium |

---

## H. Nhóm: Access Log Monitoring (Decision Table + Error Guessing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_033 | Có quyền truy cập Access Log (DB/Admin) | Login thành công (TC_LOGIN_020) | 1. Thực hiện login thành công<br>2. Kiểm tra bản ghi log mới nhất | Log ghi nhận: `Login status = success`, User ID, IP address, Login timestamp, Device/User-agent — đầy đủ theo SRS mục 4.3 | High |
| TC_LOGIN_034 | Có quyền truy cập Access Log | Login thất bại (TC_LOGIN_021) | 1. Thực hiện login sai password<br>2. Kiểm tra bản ghi log mới nhất | Log ghi nhận: `Login status = failure`, User ID (nếu xác định được), IP address, Login timestamp, Device/User-agent (nếu có) — theo SRS mục 4.2 | High |
| TC_LOGIN_035 | Có quyền truy cập Access Log | Login với email không tồn tại | 1. Login với email không tồn tại trong hệ thống<br>2. Kiểm tra log | Log vẫn ghi nhận `failure` với IP/timestamp/user-agent; xác nhận cách hệ thống xử lý khi không có User ID hợp lệ (null/giá trị nhập vào) | Medium |
| TC_LOGIN_036 | Có quyền truy cập Access Log | Login từ nhiều thiết bị/trình duyệt khác nhau | 1. Login cùng tài khoản từ 2 device khác nhau (desktop, mobile browser) | Mỗi log ghi nhận đúng Device/User-agent tương ứng, không bị ghi đè/nhầm lẫn | Low |

---

## I. Nhóm: API-level & Token Validation (Error Guessing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_LOGIN_037 | Có tool test API (Postman) | Request hợp lệ | 1. Gọi trực tiếp `POST /admin/auth/login/` với body đúng | Response 200, chứa `access_token` và `refresh_token` không rỗng, đúng định dạng JWT (nếu áp dụng) | High |
| TC_LOGIN_038 | Có tool test API | Request thiếu field `password` trong body | 1. Gọi API chỉ với `email_user_id`, thiếu `password` | Response trả lỗi CMMSG29/400, không trả token | High |
| TC_LOGIN_039 | Có tool test API | Request với method sai (`GET` thay vì `POST`) | 1. Gọi `GET /admin/auth/login/` | Response 404/405, không xử lý như login hợp lệ | Low |
| TC_LOGIN_040 | Có tool test API | Gọi login 2 lần liên tiếp cùng tài khoản, cùng credentials đúng | 1. Gọi API login 2 lần | Mỗi lần trả về `access_token`/`refresh_token` **khác nhau** (không tái sử dụng token cũ), cả 2 phiên đều hợp lệ hoặc theo đúng chính sách phiên đã định nghĩa | Medium |
| TC_LOGIN_041 | Có tool test API, server tạm ngưng hoạt động (giả lập) | Request hợp lệ nhưng server lỗi | 1. Giả lập lỗi server (503)<br>2. Gọi API login | Response trả CMMSG2 ("Service is temporarily unavailable...") thay vì crash không rõ nguyên nhân | Low |

---

## Ghi chú tổng hợp
- **CMR04** áp dụng nhất quán: không có validate FE hiển thị lỗi ngay khi nhập/rời field đối với 2 input text này; toàn bộ lỗi input đều đến từ Backend response.
- **MSG10** dùng chung cho cả 2 trường hợp "sai password" và "tài khoản không tồn tại" — đây là thiết kế bảo mật hợp lý (chống user enumeration), test case E đã xác nhận hành vi này thay vì coi là bug.
- Các trường hợp **chưa được SRS quy định rõ** (case-sensitivity của email, giới hạn độ dài field, rate-limit số lần login sai) được đánh dấu Priority Low/Medium và cần xác nhận thêm với BA/Dev trước khi coi là defect nếu kết quả thực tế khác kỳ vọng.
