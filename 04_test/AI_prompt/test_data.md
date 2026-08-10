# Mục tiêu:

Biến Test Data từ những dữ liệu QA tạo thủ công thành một artifact có thiết kế, có traceability, có coverage và có khả năng tái sử dụng cho Manual Test, API, Automation và Database Testing.

- Đảm bảo Test Case có dữ liệu để thực thi
- Tăng độ bao phủ của Test
- Tránh tạo data thủ công và thiếu tính hệ thống
- Tái sử dụng data cho nhiều loại testing
- Traceability giữa Requirement → Test Case → Test Data

# Cấu trúc prompt

1. Role: Xác định AI đóng vai trò gì
2. Context: Mô tả bối cảnh và mục đích sử dụng Test Data.
3. Task: Giao yêu cầu cho AI

- Phân tích các field và validation/business rule.
- Xác định Test Data cần thiết bằng:
  - Equivalence Partitioning
  - Boundary Value Analysis
  - Decision Table
  - Pairwise/Combination
  - Negative Testing
- Tạo data cho:
  - Valid
  - Invalid
  - Boundary
  - Empty/Blank/Null/Missing
  - Wrong Type/Format
  - Special Character/Unicode/Whitespace
  - Duplicate/Non-existing
  - Combination giữa các field
- Nếu Requirement không quy định rõ rule → đánh dấu ASSUMPTION, không tự coi là Requirement.
- Loại bỏ data trùng mục đích, tránh tạo Cartesian Product không cần thiết.
- Validate lại data về:
  - Correctness
  - Consistency
  - Format
  - Dependency
  - Coverage
- Đảm bảo data có thể tái sử dụng cho Manual Test, API, Automation và Database Testing.

4. Output: Tạo matrix data
