1. Mục tiêu: Sử dụng AI để phát hiện những yêu cầu chưa đầy đủ hoặc chưa rõ trong BRD/SRS, từ đó hỗ trợ QA chuẩn bị bộ câu hỏi trao đổi với BA/PO trước khi bắt đầu thiết kế Test Case. Mục tiêu bao gồm:

- Phát hiện Requirement còn thiếu.
- Xác định các Business Rule chưa rõ.
- Làm rõ Validation Rule.
- Kiểm tra Exception Handling.
- Làm rõ Permission.
- Xác định Error Message.
- Đặt câu hỏi về Integration và Non-functional Requirement.

2. Cấu trúc prompt

- Role (Vai trò): Xác định vai trò AI.
- Context (Bối cảnh)
- Task: Yêu cầu AI thực hiện:
  ++Tìm các Requirement chưa rõ.
  ++Đề xuất câu hỏi Clarify.
  ++Phân loại câu hỏi theo từng nhóm.
- Constraint
  ++Chỉ đặt câu hỏi dựa trên nội dung tài liệu.
  ++ Không tự tạo Requirement mới.
  ++Đánh dấu mức độ ưu tiên High, Medium, Low.

3. Kết quả mong muốn

- Danh sách các câu hỏi cần Clarify.
- Các Requirement còn thiếu hoặc mơ hồ.
- Danh sách Validation cần xác nhận.
- Các Business Rule cần làm rõ.
- Các Exception chưa được mô tả.
- Các Permission chưa đầy đủ.
- Bộ câu hỏi sẵn sàng sử dụng trong buổi Review Requirement.

Prompt
Bạn là Senior QA đã học và hiểu được dự án. Hãy đóng vai QA trong buổi Requirement Review.
Liệt kê tất cả câu hỏi cần hỏi Business Analyst. Ưu tiên phát hiện:

- Requirement thiếu
- Requirement mơ hồ
- Business Rule chưa rõ
- Validation chưa rõ
- Boundary chưa rõ
- Exception chưa rõ
- Permission chưa rõ
- Error Message chưa rõ
  Constraint
  ++ Chỉ đặt câu hỏi dựa trên nội dung tài liệu.
  ++ Không tự tạo Requirement mới.
  ++ Đánh dấu mức độ ưu tiên High, Medium, Low.
  Trình bày dưới dạng bảng rõ ràng.
