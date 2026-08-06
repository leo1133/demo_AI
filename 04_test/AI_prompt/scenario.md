1. Mục tiêu: AI hỗ trợ QA xác định phạm vi kiểm thử và các khu vực có rủi ro cao nhằm hỗ trợ lập Test Plan và ưu tiên kiểm thử.

- Xác định Test Scenario.
- Phân loại các loại kiểm thử cần thực hiện.
- Xác định Risk Area.
- Đánh giá mức độ ưu tiên.
- Xác định Regression Scope.

2. Cấu trúc Prompt

- Role (Vai trò): Xác định vai trò AI.
- Context (Bối cảnh)
- Task: Yêu cầu AI thực hiện:
  ++Liệt kê Test Scenario.
  ++Xác định Functional Test.
  ++Xác định Negative Test.
  ++Xác định Boundary Test.
  ++Xác định Integration Test.
  ++Xác định Security Test.
  ++Xác định Performance Test.
  ++Xác định Regression Scope.
  ++Xác định Risk Area.
- Constraint
  ++Chỉ dựa trên Requirement được cung cấp.
  ++Không tạo thêm chức năng ngoài tài liệu.
  ++Phân loại mức độ ưu tiên P1, P2, P3.

3. Kết quả mong muốn

- Danh sách Test Scenario theo từng chức năng.
- Danh sách các loại kiểm thử cần thực hiện (Functional, Negative, Boundary, Integration, Security, Performance).
- Danh sách các Risk Area và mức độ ảnh hưởng.
- Thứ tự ưu tiên kiểm thử (P1, P2, P3).
- Regression Scope khi có thay đổi.
- Danh sách các chức năng cần tập trung kiểm thử trước khi phát hành sản phẩm.

Prompt
Bạn là Senior QA đã học và hiểu được dự án. Hãy đóng vai QA chịu trách nhiệm lập Test Plan và phân tích Risk.

Dựa trên Requirement dưới đây, hãy xác định các kịch bản kiểm thử và khu vực có rủi ro cao.
Yêu cầu:

- Liệt kê Test Scenario.
- Xác định Functional Test.
- Xác định Negative Test.
- Xác định Boundary Test.
- Xác định Integration Test.
- Xác định Security Test.
- Xác định Performance Test.
- Xác định Regression Scope.
- Xác định Risk Area.

Constraint
++ Chỉ dựa trên Requirement được cung cấp.
++ Không tạo thêm chức năng ngoài tài liệu.
++ Phân loại mức độ ưu tiên P1, P2, P3.

Trình bày dưới dạng bảng.
