# API Testing

Mục tiêu: Làm chủ kỹ năng kiểm thử tự động ở lớp giao diện lập trình ứng dụng (API) và kết hợp API với UI để tối ưu hóa tốc độ và hiệu năng chạy test. Các kết quả cụ thể cần đạt được gồm:

- API Request: Thực hiện thành công các phương thức HTTP cơ bản bao gồm GET (lấy dữ liệu), POST (tạo mới dữ liệu), PUT (cập nhật dữ liệu), và DELETE (xóa dữ liệu) thông qua kịch bản tự động.
- API Assertions: Kiểm tra và xác minh (Validate response) các phản hồi trả về từ API như Status Code (200, 201, 400...), Response Body, Schema, và Response Time để đảm bảo API hoạt động đúng logic.
- API + UI Integration: Tích hợp API vào quá trình kiểm thử UI bằng cách sử dụng API để chuẩn bị dữ liệu kiểm thử (API setup test data) hoặc bypass các bước giao diện tốn thời gian (như Login, tạo Data) trước khi chạy UI Test.

# Hướng tiếp cận API Testing

## 1. API theo cấu trúc tương tự UI POM (Service Object Model)

Mô hình này tách biệt khai báo Request (Endpoint, Header, Payload) và kịch bản Test (Assert, Business Logic).

Ưu điểm:

- Tính tái sử dụng cao: Khai báo API một lần, dùng ở nhiều kịch bản test khác nhau.
- Dễ bảo trì: Khi API thay đổi (ví dụ: đổi route, thêm header bắt buộc), chỉ cần sửa tại một file class/object duy nhất.
- Code gọn gàng: File test chỉ tập trung vào việc truyền param và khẳng định dữ liệu (expect).

Nhược điểm: Tốn công thiết lập cấu trúc framework ban đầu (dễ bị quá tay / over-engineering nếu dự án quá nhỏ).

Đánh giá: Thích hợp với các dự án có quy mô vừa và lớn, nơi mà API được tái sử dụng ở nhiều nơi và cần sự ổn định, dễ bảo trì.

## 2. API với CSV (Data-Driven Testing)

Mô hình thiết kế các hàm test nhận tham số đầu vào và kết quả mong đợi từ file CSV.

Ưu điểm:

- Phủ kịch bản nhanh: Rất mạnh khi test Boundary, Boundary Edge, Validation (ví dụ: test 20-30 trường hợp input đúng/sai khác nhau mà không cần viết lại code).
- Dễ tiếp cận: Người không chuyên code (BA, Manual QA) có thể tự thêm kịch bản test bằng cách điền file CSV.

Nhược điểm:

- Khó xử lý API phức tạp: Không tối ưu cho API có payload dạng JSON lồng nhau nhiều tầng (nested JSON) hoặc response có cấu trúc động.
- Phụ thuộc dữ liệu tĩnh: File CSV dễ bị lỗi thời nếu dữ liệu môi trường (như ID, Token) thay đổi liên tục.

Đánh giá: Thích hợp nhất cho Boundary Testing và Negative Testing trên các API có request body đơn giản.

## 3. API kết hợp kiểm tra Database (End-to-End API Testing)

Gửi request qua Playwright API client, sau đó truy vấn trực tiếp vào Database để đối chiếu dữ liệu thực tế.

Ưu điểm:

- Đảm bảo độ chính xác cao nhất: Tránh rủi ro API trả về thành công (200 OK) nhưng dữ liệu thực tế chưa ghi xuống DB hoặc ghi sai.
- Hỗ trợ dọn dẹp data (Clean-up): Cho phép truy vấn xóa hoặc khôi phục trạng thái dữ liệu trước/sau khi chạy test (Setup/Teardown).

Nhược điểm:

- Tốc độ chạy chậm hơn: Mất thêm thời gian kết nối và chờ truy vấn DB.
- Rủi ro phụ thuộc môi trường: Cần phân quyền kết nối DB an toàn và xử lý cẩn thận để tránh làm bẩn/hỏng dữ liệu test chung trên môi trường Test/Staging.

Đánh giá: Nên có đối với các nghiệp vụ quan trọng (Create, Update, Delete, giao dịch thanh toán).

# Mô hình phù hợp với API Testing

Mô hình tối ưu nhất trong thực tế là kết hợp cả 3 phương án:

- Khung xương: Dùng Cấu trúc POM (Phương án 1) làm nền tảng tổ chức code chính.
- Nghiệp vụ cốt lõi: Áp dụng Kiểm tra DB (Phương án 3) cho các API tác động dữ liệu quan trọng.
- Phủ kịch bản: Nhúng File CSV (Phương án 2) vào các method POM để chạy Data-Driven cho các API cần validation nhiều trường dữ liệu.

## Lộ trình 4 Bước học API Testing bằng Playwright

### Bước 1: Chuẩn bị môi trường & Nắm kiến thức nền tảng

Kiến thức cốt lõi:

- Cấu trúc một HTTP Request (Method, URL, Query Params, Headers, Body) và HTTP Response (Status Code, Body JSON, Headers).
- Các thao tác JS/TS cơ bản: Async/Await, Destructuring, Import/Export, Thao tác với Object/Array.

Thực hành khởi tạo:

- Cài đặt dự án Playwright với TypeScript (tsconfig.json).
- Thực hành gọi API cơ bản bằng request.get(), request.post() trực tiếp trong file .spec.ts

### Bước 2: Chuẩn hóa Framework với API Object Model (Phương án 1)

Kiến thức cốt lõi:

- Tư duy đóng gói (Encapsulation) và kế thừa (Inheritance) trong OOP.
- Tách biệt Cấu hình/Service (src/api/) và Kịch bản kiểm thử (tests/).
- Sử dụng Custom Fixture trong Playwright để inject API Objects.

Bài tập thực hành:

- Tạo base.api.ts quản lý Base URL và headers.
- Tạo auth.api.ts (chứa hàm login) và user.api.ts (chứa các hàm getUsers, createUser).
- Xây dựng Custom Fixture api.fixture.ts cấp sẵn userAPI cho các test case.
- Viết kịch bản test user.spec.ts kiểm tra Status Code và cấu trúc JSON trả về.

### Bước 3: Mở rộng độ phủ với Data-Driven Testing bằng CSV (Phương án 2)

Kiến thức cốt lõi:

- Đọc và parse file dữ liệu tĩnh trong Node.js (dùng thư viện csv-parse hoặc papaparse).
- Kỹ thuật lặp kịch bản test (Data-Driven Testing / Parameterized Testing).

Bài tập thực hành:

- Đọc file CSV chứa dữ liệu test boundary (ví dụ: tạo user với email hợp lệ, email sai định dạng, tên để trống).
- Dùng hàm .forEach() hoặc for...of trong Playwright để tạo động các test() tương ứng với từng dòng dữ liệu trong CSV.
- Gọi method từ UserAPI ở Bước 2 để thực thi kiểm thử hàng loạt mà không bị lặp code.

### Bước 4: Kiểm tra tính toàn vẹn dữ liệu với Database (Phương án 3)

Kiến thức cốt lõi:

- Câu lệnh SQL cơ bản (SELECT, INSERT, DELETE, WHERE).
- Quản lý kết nối Database trong Node.js (sử dụng thư viện pg cho Postgres, mysql2 cho MySQL, hoặc ORM nhẹ).

Bài tập thực hành:

- Dựng một database cục bộ (hoặc qua Docker/Supabase).
- Viết db.helper.ts để kết nối và tạo hàm getUserByEmail(email) và deleteUserByEmail(email).
- Viết kịch bản End-to-End: Execute: Bắn API createUser qua UserAPI.
- Assert 1: Kiểm tra API trả về 201 Created.
- Assert 2: Gọi dbHelper.getUserByEmail() để đối chiếu xem dữ liệu thực sự đã lưu vào DB chính xác chưa.
- Clean-up: Xóa record test trong DB bằng afterEach.
