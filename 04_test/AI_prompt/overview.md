### Mục tiêu:

Giúp QA sử dụng AI để nhanh chóng nắm bắt nội dung của tài liệu BRD/SRS mà không cần đọc toàn bộ tài liệu nhiều lần.Sau bước này, QA có thể:

- Hiểu mục tiêu của dự án.
- Xác định các chức năng chính của hệ thống.
- Nhận biết các vai trò người dùng.
- Hiểu Business Flow tổng thể.
- Xác định các Business Rules quan trọng.
- Nhận diện các tích hợp (Integration) với hệ thống khác.
- Nắm được các yêu cầu phi chức năng (Non-functional Requirements).
- Chuẩn bị kiến thức nền trước khi thực hiện Clarify Requirement và thiết kế Test Case.

### Cấu trúc Prompt

- Role (Vai trò): Xác định vai trò AI.
- Context (Bối cảnh)
- Task: Yêu cầu AI thực hiện:
  ++ Tóm tắt Project Overview.
  ++ Xác định mục tiêu dự án.
  ++ Liệt kê User Roles.
  ++ Liệt kê Main Modules.
  ++ Mô tả Business Flow.
  ++ Trích xuất Business Rules.
  ++ Xác định Integration.
  ++ Tóm tắt Non-functional Requirements.
- Constraint (Ràng buộc)
  ++ Không suy diễn ngoài tài liệu.
  ++Nếu thiếu thông tin thì ghi rõ "Chưa được đề cập".
  ++Chỉ sử dụng thông tin có trong BRD/SRS.
- Output Format: Yêu cầu AI trình bày theo Markdown hoặc bảng.

### Kết quả mong muốn

- Bản tóm tắt ngắn gọn của toàn bộ dự án.
- Danh sách các module chính.
- Business Flow tổng quan.
- Business Rules quan trọng.
- Các yêu cầu phi chức năng.

Prompt
Bạn là Senior Business Analyst và Senior QA. Trong toàn bộ cuộc hội thoại này, bạn sẽ trở thành một thành viên của dự án và học toàn bộ tài liệu SRS được tôi cung cấp.
Hãy đọc toàn bộ tài liệu và tóm tắt:

1. Project Goal
2. Business Objective
3. User Roles
4. Main Modules
5. Business Flow
6. Business Rules
7. Integration
8. Non-functional Requirement

Constraint (Ràng buộc)
++ Không suy diễn ngoài tài liệu.
++ Nếu thiếu thông tin thì ghi rõ "Chưa được đề cập".
++ Chỉ sử dụng thông tin có trong BRD/SRS.

Trình bày dưới dạng Markdown rõ ràng.
