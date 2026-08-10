# Mục tiêu:

- Đánh giá xem bộ Test Case hiện tại có thực sự kiểm thử đủ những gì hệ thống cần kiểm thử hay chưa

* Biết Requirement nào đã được cover
* Tìm Test Case bị thiếu
* Kiểm tra "độ sâu" của Test Case
* Phát hiện những góc nhìn mà người viết Test Case bỏ sót
* Giúp quyết định có thể sử dụng bộ Test Case hay cần bổ sung Test Case

# Cấu trúc prompt

1. Role (Vai trò)
2. Context (Bối cảnh)
3. Task: Yêu cầu AI thực hiện:

- Extract toàn bộ Requirement có thể kiểm thử.
- Mapping Requirement ↔ Test Case.
- Xác định FULLY COVERED / PARTIALLY COVERED / NOT COVERED.
- Tìm các case còn thiếu về Business Rule, Validation, Negative, Boundary, Workflow/State, Combination, Error Handling và Viewpoint/Technique phù hợp.
- Phát hiện Test Case trùng lặp hoặc không tạo thêm coverage.
- Tính Requirement Coverage %.
- Đề xuất Missing Test Cases và Priority.

4. Constraint:

- Không tự bịa Requirement.
- Phân biệt EXPLICIT / INFERRED / RECOMMENDED.
- Nếu SRS thiếu thông tin, ghi INSUFFICIENT INFORMATION.

5. Output: Xác định mức độ bao phủ của Test Case hiện tại và tìm ra các khoảng trống kiểm thử.

- Coverage Summary
- Requirement Traceability Matrix
- Missing Test Cases
- Redundant Test Cases
- Final Assessment
