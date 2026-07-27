# BẢN TÀI LIỆU YÊU CẦU PHẦN MỀM (SRS) & TÓM TẮT HỆ THỐNG CRM

**URL Hệ thống:** `https://crm.anhtester.com/admin/authentication`  
**Tài khoản demo:** `admin@example.com` / `123456`  
**Tên hệ thống:** Perfex CRM / Anh Tester CRM System  
**Phiên bản:** 1.0  
**Ngày tạo:** 27s/07/2026

---

### 1. Giới thiệu (Introduction)

#### 1.1 Mục đích (Purpose)

Tài liệu này xác định các yêu cầu chức năng và phi chức năng cho Hệ thống Quản lý Quan hệ Khách hàng (CRM) nhằm phục vụ công tác phát triển, bảo trì và xây dựng kịch bản kiểm thử (Test Cases).

#### 1.2 Phạm vi hệ thống (Scope)

Hệ thống CRM cung cấp nền tảng quản lý tập trung cho doanh nghiệp nhỏ và vừa, xử lý từ khâu thu hút khách hàng tiềm năng, chốt hợp đồng, triển khai dự án cho đến xuất hóa đơn tài chính và hỗ trợ kỹ thuật.

---

### 2. Mô tả tổng quan (Overall Description)

#### 2.1 Các nhóm người dùng (User Classes and Characteristics)

- **System Administrator (Admin):** Toàn quyền truy cập, cấu hình hệ thống, quản lý tài khoản nhân viên, xem báo cáo tổng quan.
- **Staff / Employee (Nhân viên):** Thực hiện tác vụ chuyên môn (quản lý lead, chăm sóc khách hàng, cập nhật task dự án, tạo báo giá).
- **Customer / Client (Khách hàng):** Truy cập Portal riêng để theo dõi tiến độ dự án, xem/thanh toán hóa đơn, gửi ticket hỗ trợ.

#### 2.2 Môi trường vận hành (Operating Environment)

- **Frontend:** Web responsive tương thích với các trình duyệt hiện đại (Chrome, Edge, Firefox, Safari).
- **Backend:** PHP / MySQL.
- **Giao thức:** HTTPS.

---

### 3. Yêu cầu Chức năng (Functional Requirements)

#### 3.1 Phân hệ Xác thực & Hệ thống (Authentication & System)

- **FR-01 (Login/Logout):** Cho phép người dùng đăng nhập qua Email/Password. Hỗ trợ chức năng ghi nhớ đăng nhập ("Remember Me") và quên mật khẩu.
- **FR-02 (Role-based Authorization):** Giới hạn quyền truy cập menu và chức năng dựa trên vai trò được gán trong hệ thống.
- **FR-03 (Staff Management):** Tạo mới, chỉnh sửa, vô hiệu hóa tài khoản nhân viên; phân bổ phòng ban.

#### 3.2 Phân hệ Quản lý Khách hàng & Tiềm năng (Customers & Leads)

- **FR-04 (Customer Management):**
  - Thêm, sửa, xóa, tìm kiếm thông tin doanh nghiệp/cá nhân khách hàng.
  - Quản lý danh sách người liên hệ (Contacts) thuộc khách hàng.
- **FR-05 (Lead Management):**
  - Theo dõi khách hàng tiềm năng theo trạng thái (New, Contacted, Qualified, Lost).
  - Chuyển đổi Lead thành Customer chính thức khi chốt hợp đồng.

#### 3.3 Phân hệ Quản lý Dự án & Công việc (Projects & Tasks)

- **FR-06 (Project Tracking):**
  - Tạo dự án, gán nhân sự phụ trách, thiết lập ngày bắt đầu/kết thúc.
  - Theo dõi tiến độ dự án theo tỷ lệ phần trăm hoàn thành.
- **FR-07 (Task Management):**
  - Tạo và phân công công việc cụ thể trong dự án.
  - Thiết lập mức độ ưu tiên (Low, Medium, High, Urgent), deadline và trạng thái task.
  - Tích hợp bộ đếm giờ (Timesheet Tracker) để ghi nhận thời gian làm việc.

#### 3.4 Phân hệ Tài chính & Hợp đồng (Financials & Contracts)

- **FR-08 (Estimates & Proposals):** Soạn thảo báo giá/đề xuất gửi khách hàng phê duyệt trực tuyến.
- **FR-09 (Invoices & Payments):**
  - Tạo hóa đơn tự động từ dự án hoặc tạo thủ công.
  - Ghi nhận lịch sử thanh toán và theo dõi công nợ quá hạn.
- **FR-10 (Contracts):** Lưu trữ và quản lý hợp đồng điện tử, chu kỳ gia hạn hợp đồng.

#### 3.5 Phân hệ Hỗ trợ Khách hàng (Support Tickets)

- **FR-11 (Ticket System):**
  - Tiếp nhận yêu cầu hỗ trợ từ khách hàng.
  - Phân loại ticket theo mức độ ưu tiên, phòng ban và gán nhân viên xử lý.
  - Lưu trữ lịch sử trao đổi giữa nhân viên và khách hàng.

---

### 4. Yêu cầu Phi chức năng (Non-Functional Requirements)

#### 4.1 Hiệu năng (Performance Requirements)

- **NPH-01:** Thời gian phản hồi trang chính (Dashboard) dưới 2 giây đối với kết nối tiêu chuẩn.
- **NPH-02:** Hỗ trợ tối thiểu 100 người dùng thao tác đồng thời không gây giảm hiệu năng hệ thống.

#### 4.2 Bảo mật (Security Requirements)

- **SEC-01:** Mật khẩu người dùng phải được mã hóa theo chuẩn an toàn (bcrypt/argon2).
- **SEC-02:** Tự động hết hạn phiên làm việc (session timeout) sau 30 phút không có thao tác.
- **SEC-03:** Ngăn chặn các lỗ hổng bảo mật phổ biến (SQL Injection, XSS, CSRF).

#### 4.3 Khả năng sử dụng (Usability Requirements)

- **USA-01:** Giao diện trực quan, nhất quán về định dạng màu sắc, biểu tượng và font chữ.
- **USA-02:** Hiển thị thông báo rõ ràng khi thao tác thành công hoặc phát sinh lỗi (Validation Messages).

---

### 5. Ma trận Kiểm thử Chức năng Chính (Sample Traceability Matrix)

| Mã yêu cầu | Chức năng | Kịch bản kiểm thử trọng tâm                                      |
| :--------- | :-------- | :--------------------------------------------------------------- |
| **FR-01**  | Login     | Kiểm thử đăng nhập đúng/sai credentials, kiểm tra thông báo lỗi. |
| **FR-04**  | Customers | Kiểm thử thêm mới khách hàng, tìm kiếm, lọc theo điều kiện.      |
| **FR-07**  | Tasks     | Kiểm thử chuyển đổi trạng thái task, ghi nhận log thời gian.     |
| **FR-09**  | Invoices  | Kiểm thử tính toán tổng tiền, thuế và xuất file PDF hóa đơn.     |
