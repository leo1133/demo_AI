
# Mục tiêu:

- Yêu cầu AI phân tích từng requirement đã extract từ SRS.
- Tự lựa chọn kỹ thuật Test Design phù hợp:
  + Equivalence Partitioning (EP)
  + Boundary Value Analysis (BVA)
  + Decision Table
  + State Transition
  + Use Case Testing
  + Pairwise Testing
  - Error Guessing
- Sinh Test Case có coverage tốt.
- Tránh sinh test case trùng lặp hoặc chỉ dựa vào happy path.


# Cấu trúc prompt

1. Role (Vai trò): Xác định vai trò AI.
2. Context (Bối cảnh)
3. Task: Yêu cầu AI thực hiện:
- Phân tích từng requirement.
- Xác định loại logic cần kiểm thử:
  + Input validation
  + Business rule
  + Workflow
  + State change
  + Combination condition
  + Data variation

- Chọn kỹ thuật Test Design phù hợp:
  + Equivalence Partitioning
  + Boundary Value Analysis
  + Decision Table Testing
  + State Transition Testing
  + Use Case Testing
  + Pairwise Testing
  + Error Guessing

- Giải thích lý do chọn kỹ thuật.
- Sinh Test Case chi tiết.

4. Output: File testcase gồm các trường
  | Test Case ID |
  | Preconditions |
  | Test Data |
  | Test Steps |
  | Expected Result |
  | Priority |


# Kết quả mong muốn
   AI tạo ra:

- Mapping Requirement → Test Design Technique
- Test Case chi tiết

# Prompt
Bạn là Senior QA Engineer / Test Architect.

Bạn đã phân tích SRS và có danh sách Requirement như file tôi đính kèm

Nhiệm vụ:

1. Phân tích từng requirement.
2. Xác định loại logic cần kiểm thử:
   - Input validation
   - Business rule
   - Workflow
   - State change
   - Combination condition
   - Data variation

3. Chọn kỹ thuật Test Design phù hợp:
   - Equivalence Partitioning
   - Boundary Value Analysis
   - Decision Table Testing
   - State Transition Testing
   - Use Case Testing
   - Pairwise Testing
   - Error Guessing

4. Giải thích lý do chọn kỹ thuật.

5. Sinh Test Case chi tiết với output là file .md với các trường: 

| Test Case ID |
| Preconditions |
| Test Data |
| Test Steps |
| Expected Result |
| Priority |

# Demo

**Phân tích Test Design — AA000_Login**
Tôi đã đối chiếu SRS với Common_Spec.md (CMR03, CMR04, CMR41...) và Common_MSG.md/MSG.md (CMMSG29, MSG10) để đảm bảo test case bám đúng message code và business rule thực tế, không suy đoán.
1–3. Phân tích Requirement, Loại logic & Kỹ thuật Test Design
### PHÂN TÍCH TEST DESIGN — Màn hình AA000_Login (Đăng nhập Hệ thống)

**Nguồn tham chiếu:** `srs_summary_login.md`, `Common_Spec.md` (CMR03, CMR04, CMR41), `Common_MSG.md` (CMMSG29), `MSG.md` (MSG10)

---

## 1–3. Phân tích Requirement, Loại logic & Kỹ thuật Test Design

| # | Requirement | Loại logic | Kỹ thuật chọn | Lý do |
|---|---|---|---|---|
| R1 | Field Email/Login ID (bắt buộc, free-text, CMR04) | Input validation | Equivalence Partitioning, Error Guessing | CMR04 quy định **không có validate FE với error message** cho text field (chỉ block ký tự ở numeric field) → cần EP để xác nhận field chấp nhận mọi input hợp lệ/không hợp lệ và **không** hiện lỗi FE ngay; Error Guessing để phát hiện các input "khó lường" (khoảng trắng đầu/cuối, ký tự đặc biệt, chuỗi cực dài) |
| R2 | Field Password (bắt buộc, ẩn ký tự mặc định) | Input validation + UI state | Equivalence Partitioning, Error Guessing | Tương tự R1, thêm kiểm tra riêng về việc masking (`********`) — đây là state hiển thị chứ không phải rule dữ liệu, nên tách EP (giá trị) khỏi kiểm tra UI masking |
| R3 | Required field check (CMR41 → CMMSG29) | Business rule | Decision Table Testing | Có nhiều tổ hợp trạng thái rỗng/không rỗng giữa 2 field (email rỗng, password rỗng, cả hai rỗng, cả hai có dữ liệu) → Decision Table biểu diễn đầy đủ và tránh trùng lặp |
| R4 | Xác thực tài khoản (đúng/sai) → MSG10 | Business rule | Equivalence Partitioning (tài khoản tồn tại/không tồn tại, đúng/sai mật khẩu) + Decision Table | Kết quả phụ thuộc tổ hợp (email đúng/sai) × (password đúng/sai) → Decision Table; đồng thời phân lớp tương đương cho "tài khoản không tồn tại" vs "tài khoản tồn tại nhưng sai mật khẩu" vì SRS gộp chung MSG10 cho cả 2 case |
| R5 | Nút Login → gọi API, điều hướng Home / hiển thị lỗi | Workflow + State change | Use Case Testing, State Transition Testing | Đây là luồng end-to-end nhiều bước (nhập → submit → response → điều hướng/hiển thị lỗi) với trạng thái rõ ràng: Unauthenticated → (Submitting) → Authenticated/Failed → State Transition mô tả chính xác các state & transition hợp lệ |
| R6 | Nút "パスワードを忘れた方" → AB001 | Workflow | Use Case Testing | Một hành động điều hướng đơn giản, không có logic điều kiện phức tạp — Use Case đủ để cover happy path + việc giữ nguyên dữ liệu đã nhập (nếu có) |
| R7 | Access Log (success/failure) ghi User ID, IP, timestamp, device | Data variation / Business rule | Decision Table Testing, Error Guessing | Nội dung log thay đổi theo kết quả login (success log đủ field, failure log theo field khác) → Decision Table cho 2 nhánh; Error Guessing cho case thiếu User-Agent, IP không xác định được |
| R8 | UX: nút Login không disable khi field rỗng, lỗi chỉ hiện sau response thất bại | State change | State Transition Testing | Đặc thù dễ bị hiểu sai (nhiều SRS khác disable nút) → cần test riêng transition "Empty fields → Click → Error shown" để tránh regression |
| R9 | Security: mã hoá password qua HTTPS, không log plaintext | Business rule (non-functional) | Error Guessing | Không có input logic để EP/BVA áp dụng; đây là rủi ro bảo mật kinh điển cần kinh nghiệm QA để guess (network sniffing, log file inspection) |
| R10 | Response cấp `access_token` & `refresh_token` | Data variation / Business rule | Equivalence Partitioning, Error Guessing | Kiểm tra token hợp lệ, không rỗng, không trùng lặp giữa các lần login, và test negative khi response thiếu token |

---

### Lưu ý riêng cho R1/R2 (theo CMR04)

Vì rule quy định *"No frontend validation with error messages"* và *"If the field is empty → still enable related button, error đến từ backend"*, nên **không áp dụng Boundary Value Analysis cho FE** (không có giới hạn ký tự công bố cụ thể trong SRS/Common Spec để BVA), thay vào đó dùng Equivalence Partitioning ở tầng dữ liệu gửi lên **Backend/API** kết hợp Error Guessing cho input bất thường.