### Prompt là gì?

Định nghĩa: Prompt là đoạn mô tả hoặc câu lệnh bạn gửi cho AI để yêu cầu AI thực hiện một nhiệm vụ.
Các tiêu chí của Prompt hiệu quả:

- Rõ ràng (Clear)
- Có bối cảnh (Context)
- Nhiệm vụ cụ thể (Task)
- Có ràng buộc (Constraints)
- Chỉ định định dạng kết quả (Output format)
- Xác định vai trò AI (Role)
- Ngắn gọn nhưng đủ thông tin

### Cấu trúc của một Prompt chuẩn

Prompt hiệu quả cần có 5 thành phần như sau:

- Role
- Context
- Task
- Constraints
- Output format

1. Role – Xác định vai trò của AI

- Role là việc yêu cầu AI đóng vai một chuyên gia hoặc vị trí cụ thể trước khi thực hiện nhiệm vụ.
- Việc chỉ định Role giúp AI:

* Chọn góc nhìn chuyên môn phù hợp

* Áp dụng cách thức đúng và phù hợp nhất

* Đưa ra giải pháp thực tế hơn
  => Khi có Role, AI thường viết code clean hơn và đúng chuẩn hơn.

2. Context – Cung cấp bối cảnh hệ thống

- AI không biết hệ thống của bạn là gì => Vì vậy cần cung cấp Context để AI hiểu môi trường làm việc.

3. Task

- Là phần quan trọng nhất của Prompt. Đây là nơi bạn nói rõ AI cần làm gì.
- Task nên:

* cụ thể
* rõ ràng
* không mơ hồ

4. Constraints – Đặt các ràng buộc

- Là các quy tắc hoặc giới hạn mà AI phải tuân theo

5. Output Format - Định dạng kết quả

- Yêu cầu AI trình bày kết quả theo cấu trúc rõ ràng.

6. Ví dụ về prompt
   6.1. prompt yêu cầu AI tóm tắt overview dự án từ tài liệu SRS/BRD
   Bạn là Senior Business Analyst và Senior QA. Trong toàn bộ cuộc hội thoại này, bạn sẽ trở thành một thành viên của dự án và học toàn bộ tài liệu SRS được tôi cung cấp.
   Hãy đọc toàn bộ tài liệu và tóm tắt:
   ++ Project Goal
   ++ Business Objective
   ++ User Roles
   ++ Main Modules
   ++ Business Flow
   ++ Business Rules
   ++ Integration
   ++ Non-functional Requirement( nếu có trong tài liệu)
   Constraint (Ràng buộc)
   ++ Không suy diễn ngoài tài liệu.
   ++ Nếu thiếu thông tin thì ghi rõ "Chưa được đề cập".
   ++ Chỉ sử dụng thông tin có trong BRD/SRS.
   Out put: Trình bày dưới dạng Markdown rõ ràng.

   6.2. prompt yêu cầu AI tạo bộ câu hỏi Q&A từ tài liệu SRS.BRD
   Bạn là Senior QA đã học và hiểu được dự án. Hãy đóng vai QA trong buổi Requirement Review.
   Liệt kê tất cả câu hỏi cần hỏi Business Analyst. Ưu tiên phát hiện:
   ++ Requirement thiếu
   ++ Requirement mơ hồ
   ++ Business Rule chưa rõ
   ++ Validation chưa rõ
   ++ Boundary chưa rõ
   ++ Exception chưa rõ
   ++ Permission chưa rõ
   ++ Error Message chưa rõ
   Constraint
   ++Chỉ đặt câu hỏi dựa trên nội dung tài liệu.
   ++ Không tự tạo Requirement mới.
   ++Đánh dấu mức độ ưu tiên High, Medium, Low.
   Out put: Trình bày dưới dạng bảng rõ ràng.

   6.3. prompt yêu cầu AI xác định Test Scenario và Risk Area
   Bạn là Senior QA đã học và hiểu được dự án. Hãy đóng vai QA chịu trách nhiệm lập Test Plan và phân tích Risk.
   Dựa trên Requirement dưới đây, hãy xác định các kịch bản kiểm thử và khu vực có rủi ro cao.
   Yêu cầu:
   ++ Liệt kê Test Scenario.
   ++ Xác định Functional Test.
   ++ Xác định Negative Test.
   ++ Xác định Boundary Test.
   ++ Xác định Integration Test.
   ++ Xác định Security Test.
   ++ Xác định Performance Test.
   ++ Xác định Regression Scope.
   ++ Xác định Risk Area.
   Constraint
   ++ Chỉ dựa trên Requirement được cung cấp.
   ++ Không tạo thêm chức năng ngoài tài liệu.
   ++ Phân loại mức độ ưu tiên P1, P2, P3.
   Out put: Trình bày dưới dạng file markdown.

   6.4. prompt yêu cầu AI viết testcase dựa trên tài liệu SRS/BRD ( case dựa trên file viewpoint )
   Đóng vai trò là 1 QA có kinh nghiệm trong domain E-Commerce. Trong toàn bộ cuộc hội thoại này, bạn sẽ trở thành một thành viên của dự án và học toàn bộ tài liệu tôi cung cấp. Dựa vào file tài liệu SRS và viewpoint tôi cung cấp, hãy thực hiện
   ++ Viết testcase cho tính năng Đăng ký
   ++ File testcase có format: ID, Testcase name, Sub testcase name, Pre-condition, Steps, Expected result Quy tắc
   ++ Giữ nguyên cách chia testcase trong file viewpoint
   ++ Testcase check lần lượt: UI/UX, Validation, Business
   ++ Chỉ sử dụng thông tin trong tài liệu hoặc thông tin tôi cung cấp.
   ++ Không tự suy diễn.
   Out put: tạo file testcase có tên "register.md" và lưu vào thư mục /Users/ngaphuong/projects/demo_AI/04_test/testcase

   6.5. prompt yêu cầu AI viết testcase dựa trên tài liệu SRS/BRD ( case không có viewpoint )
