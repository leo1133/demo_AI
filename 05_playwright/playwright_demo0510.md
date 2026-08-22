# API Testing

Mục tiêu: chủ kỹ năng kiểm thử tự động ở lớp giao diện lập trình ứng dụng (API) và kết hợp API với UI để tối ưu hóa tốc độ và hiệu năng chạy test. Các kết quả cụ thể cần đạt được gồm:

- API Request: Thực hiện thành công các phương thức HTTP cơ bản bao gồm GET (lấy dữ liệu), POST (tạo mới dữ liệu), PUT (cập nhật dữ liệu), và DELETE (xóa dữ liệu) thông qua kịch bản tự động.
- API Assertions: Kiểm tra và xác minh (Validate response) các phản hồi trả về từ API như Status Code (200, 201, 400...), Response Body, Schema, và Response Time để đảm bảo API hoạt động đúng logic.
- API + UI Integration: Tích hợp API vào quá trình kiểm thử UI bằng cách sử dụng API để chuẩn bị dữ liệu kiểm thử (API setup test data) hoặc bypass các bước giao diện tốn thời gian (như Login, tạo Data) trước khi chạy UI Test.
