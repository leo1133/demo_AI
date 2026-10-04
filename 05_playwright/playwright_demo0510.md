# 🚀 Step 4: API Testing & Automation (Playwright)

Tài liệu hướng dẫn học và làm API Testing một cách đơn giản, dễ hiểu và bám sát dự án thực tế.

---

## 🎯 1. Mục Tiêu

1. **Gửi được các loại API Request**: Biết cách gọi `GET`, `POST`, `PUT`, `DELETE`.
2. **Kiểm tra phản hồi (API Assertions)**: Tự động kiểm tra xem server trả về đúng dữ liệu, đúng mã lỗi và đúng cấu trúc hay không.
3. **Kết hợp API + UI (API Setup Test Data)**: Dùng API để tạo sẵn dữ liệu trong 1 giây trước khi chạy UI test (thay vì phải bấm tay trên màn hình rất chậm).
4. **Review kết quả**: Xem báo cáo kiểm thử (HTML Report) để biết test case nào Pass/Fail và lý do lỗi.

---

## 📋 2. Các Step Thực Hiện

### 🔹 Bước 1: Gửi API Request (`GET`, `POST`, `PUT`, `DELETE`)

- **Làm gì**: Dùng `Playwright APIRequestContext` để gửi request lên server.
- **Cách làm**:
  - Tạo file cấu hình đường dẫn `endpoint.js` (ví dụ: `/api/users`, `/api/login`).
  - Gom các hàm gọi API vào các file riêng (`AuthAPI.js`, `UserAPI.js`) để code gọn gàng, tái sử dụng được nhiều lần.
  - Thử nghiệm đủ 4 hành động: `GET` (lấy danh sách), `POST` (tạo mới), `PUT` (chỉnh sửa), `DELETE` (xóa).

---

### 🔹 Bước 2: Kiểm tra dữ liệu trả về (API Assertions - Validate Response)

- **Làm gì**: Viết các câu lệnh kiểm tra (`expect`) để đảm bảo API hoạt động chuẩn xác.
- **Kiểm tra 4 thứ quan trọng**:
  1. **Mã trạng thái (Status Code)**: Thành công phải trả `200/201`; Không có quyền phải trả `401/403`; Sai dữ liệu phải trả `400`.
  2. **Cấu trúc JSON (Schema)**: Kiểm tra có đủ các trường không (ví dụ: `id` phải là số, `name` phải là chữ, `created_at` phải có định dạng ngày).
  3. **Kiểm tra dữ liệu chi tiết**: Kiểm tra đúng phân trang (`page`, `total_count`), dữ liệu tìm kiếm đúng từ khóa.
  4. **Đối chiếu với Database (PostgreSQL)**: Lấy dữ liệu API trả về, so sánh trực tiếp với dữ liệu trong Database xem có khớp 100% không.

---

### 🔹 Bước 3: Tích hợp API vào UI Test (API Setup Test Data)

- **Vấn đề**: Khi test UI tính năng "Xóa User", nếu màn hình chưa có User nào thì test sẽ fail. Nếu bấm tay trên giao diện để tạo User thì mất 10 - 20 giây.
- **Giải pháp (API Setup Data)**:
  - **Trước khi test UI (Pre-condition)**: Gọi API `POST /users` tạo ngay 1 user mới chỉ trong 0.1 giây.
  - **Test UI**: Mở trình duyệt, tìm đúng user vừa tạo và bấm nút "Xóa" trên giao diện.
  - **Sau khi test xong (Clean-up)**: Gọi API `DELETE /users/:id` để dọn sạch dữ liệu, không để lại rác trên hệ thống.

---

### 🔹 Bước 4: Tối ưu hoá nâng cao (Lưu Token & Test hàng loạt từ CSV)

- **Lưu sẵn Token Đăng nhập (Token Cache)**: Đăng nhập 1 lần và lưu token lại để dùng chung cho mọi bài test, không cần login đi login lại gây chậm máy.
- **Test hàng loạt bằng file CSV (Data-Driven)**: Điền hàng chục trường hợp (trang âm, text rỗng, ký tự lạ...) vào 1 file Excel/CSV, hệ thống sẽ tự động đọc và chạy hết toàn bộ.

---

### 🔹 Bước 5: Review Kết Quả (Review kết quả: Step 4)

- **Làm gì**: Mở báo cáo sau khi chạy test bằng lệnh `npx playwright show-report`.
- **Đánh giá**:
  - Bao nhiêu test case **Pass** (Xanh), bao nhiêu test case **Fail** (Đỏ).
  - Xem log chi tiết, thời gian phản hồi (Response time) và ảnh chụp lỗi (nếu có lỗi trên UI).

---

## 🏆 3. Kết Quả Đạt Được

1. **Hiểu rõ cách test API**: Tự tin viết được kịch bản kiểm thử cho mọi loại API (`GET, POST, PUT, DELETE`).
2. **Bắt lỗi chính xác**: Phát hiện ngay khi backend trả sai dữ liệu, sai status code hoặc sai database.
3. **UI Test chạy siêu nhanh**: Giảm thời gian chạy kiểm thử UI nhờ dùng API để tạo và dọn dẹp dữ liệu tự động.
4. **Báo cáo chuyên nghiệp**: Có báo cáo kiểm thử trực quan, rõ ràng để đánh giá chất lượng sản phẩm.
