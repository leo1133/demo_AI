1.  Mục tiêu:

- Yêu cầu AI phân tích từng requirement đã extract từ SRS.
- Tự lựa chọn kỹ thuật Test Design phù hợp:
  - Equivalence Partitioning (EP)
  - Boundary Value Analysis (BVA)
  - Decision Table
  - State Transition
  - Use Case Testing
  - Pairwise Testing
  - Error Guessing
- Sinh Test Case có coverage tốt.
- Tránh sinh test case trùng lặp hoặc chỉ dựa vào happy path.

2. Cấu trúc prompt

- Role (Vai trò): Xác định vai trò AI.
- Context (Bối cảnh)
- Task: Yêu cầu AI thực hiện:
  ++ Phân tích từng requirement.
  ++ Xác định loại logic cần kiểm thử:
  +++ Input validation
  +++ Business rule
  +++ Workflow
  +++ State change
  +++ Combination condition
  +++ Data variation

  ++ Chọn kỹ thuật Test Design phù hợp:
  +++ Equivalence Partitioning
  +++ Boundary Value Analysis
  +++ Decision Table Testing
  +++ State Transition Testing
  +++ Use Case Testing
  +++ Pairwise Testing
  +++ Error Guessing

  ++ Giải thích lý do chọn kỹ thuật.
  ++ Sinh Test Case chi tiết.

- Output
  | Test Case ID |
  | Preconditions |
  | Test Data |
  | Test Steps |
  | Expected Result |
  | Priority |

3. Kết quả mong muốn
   AI tạo ra:

- Mapping Requirement → Test Design Technique
- Test Case chi tiết

Prompt
Bạn là Senior QA Engineer / Test Architect.

Bạn đã phân tích SRS và có danh sách Requirement như sau:

[Insert AI analyzed SRS]

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

5. Sinh Test Case chi tiết.

Yêu cầu output:

| Test Case ID |
| Preconditions |
| Test Data |
| Test Steps |
| Expected Result |
| Priority |
