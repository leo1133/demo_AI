# Test Case - UC01: Đăng ký tài khoản (Register)

**Đối tượng kiểm thử:** Chức năng Đăng ký thành viên (Website Mua sắm Trực tuyến)
**Kỹ thuật áp dụng:** Equivalence Partitioning (EP), Boundary Value Analysis (BVA), Decision Table Testing, State Transition Testing, Use Case Testing, Error Guessing

---

## A. Nhóm Happy Path / Workflow (Use Case Testing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_001 | Người dùng chưa có tài khoản, đang ở trang Đăng ký | Họ tên: "Nguyen Van A"; Email: "vana.test01@gmail.com"; SĐT: "0912345678"; Mật khẩu: "Abc@1234"; Nhập lại MK: "Abc@1234" | 1. Nhập đầy đủ, hợp lệ các trường.<br>2. Nhấn nút "Đăng ký" | Hệ thống tạo tài khoản mới thành công, gửi email kích hoạt, hiển thị thông báo "Đăng ký thành công" | High |
| TC_REG_025 | Đã thực hiện đăng ký thành công ở TC_REG_001 | Email vừa đăng ký | 1. Kiểm tra hộp thư email vừa đăng ký | Nhận được email kích hoạt tài khoản đúng địa chỉ, chứa link/hướng dẫn kích hoạt | High |

---

## B. Trường Họ và tên (EP + BVA)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_002 | Đang ở form Đăng ký | Họ tên: "" (bỏ trống), các trường khác hợp lệ | 1. Để trống Họ tên.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi trường Họ tên là bắt buộc, không cho đăng ký | High |
| TC_REG_003 | Đang ở form Đăng ký | Họ tên đúng 50 ký tự, các trường khác hợp lệ | 1. Nhập Họ tên = 50 ký tự.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống chấp nhận, đăng ký thành công (biên hợp lệ) | Medium |
| TC_REG_004 | Đang ở form Đăng ký | Họ tên = 51 ký tự, các trường khác hợp lệ | 1. Nhập Họ tên = 51 ký tự.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi vượt quá độ dài cho phép (biên không hợp lệ) | Medium |
| TC_REG_028 | Đang ở form Đăng ký | Họ tên: "   Nguyen Van A   " (có khoảng trắng đầu/cuối) | 1. Nhập Họ tên có khoảng trắng thừa.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống tự động trim khoảng trắng và đăng ký thành công, hoặc báo lỗi rõ ràng nếu không hỗ trợ | Low |

---

## C. Trường Email (EP + Business Rule + Error Guessing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_005 | Đang ở form Đăng ký | Email: "" (bỏ trống) | 1. Để trống Email.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi Email là bắt buộc | High |
| TC_REG_006 | Đang ở form Đăng ký | Email: "vana.gmail.com" (thiếu ký tự @) | 1. Nhập Email sai định dạng.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi định dạng Email không hợp lệ | High |
| TC_REG_007 | Trong hệ thống đã tồn tại tài khoản với email "vana.test01@gmail.com" | Email: "vana.test01@gmail.com" (email đã tồn tại) | 1. Nhập Email đã tồn tại.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi "Email hoặc Số điện thoại đã được đăng ký" | High |

---

## D. Trường Số điện thoại (EP + BVA + Decision Table)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_008 | Đang ở form Đăng ký | SĐT: "" (bỏ trống) | 1. Để trống SĐT.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi SĐT là bắt buộc | High |
| TC_REG_009 | Đang ở form Đăng ký | SĐT: "091234567" (9 ký tự, thiếu 1 số) | 1. Nhập SĐT chỉ có 9 ký tự.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi SĐT không đúng độ dài (biên dưới không hợp lệ) | High |
| TC_REG_010 | Đang ở form Đăng ký | SĐT: "0212345678" (đầu số 02x không thuộc danh sách hợp lệ) | 1. Nhập SĐT với đầu số không hợp lệ (02x/01x/04x/06x).<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi định dạng SĐT không hợp lệ | High |
| TC_REG_011 | Trong hệ thống đã tồn tại tài khoản với SĐT "0912345678" | SĐT: "0912345678" (đã tồn tại) | 1. Nhập SĐT đã tồn tại.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi "Email hoặc Số điện thoại đã được đăng ký" | High |
| TC_REG_012a | Đang ở form Đăng ký | SĐT đầu số hợp lệ đại diện: "0312345678" (03x) | 1. Nhập SĐT với đầu số 03x, đủ 10 ký tự.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống chấp nhận, đăng ký thành công | Medium |
| TC_REG_012b | Đang ở form Đăng ký | SĐT đầu số hợp lệ đại diện: "0512345678" (05x) | Tương tự TC_REG_012a với đầu số 05x | Hệ thống chấp nhận, đăng ký thành công | Medium |
| TC_REG_012c | Đang ở form Đăng ký | SĐT đầu số hợp lệ đại diện: "0712345678" (07x) | Tương tự TC_REG_012a với đầu số 07x | Hệ thống chấp nhận, đăng ký thành công | Medium |
| TC_REG_012d | Đang ở form Đăng ký | SĐT đầu số hợp lệ đại diện: "0812345678" (08x) | Tương tự TC_REG_012a với đầu số 08x | Hệ thống chấp nhận, đăng ký thành công | Medium |
| TC_REG_012e | Đang ở form Đăng ký | SĐT đầu số hợp lệ đại diện: "0912345670" (09x) | Tương tự TC_REG_012a với đầu số 09x | Hệ thống chấp nhận, đăng ký thành công | Medium |

---

## E. Trường Mật khẩu (BVA + Decision Table - tổ hợp điều kiện)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_013 | Đang ở form Đăng ký | Mật khẩu: "" (bỏ trống) | 1. Để trống Mật khẩu.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi Mật khẩu là bắt buộc | High |
| TC_REG_014 | Đang ở form Đăng ký | Mật khẩu: "Abc@123" (7 ký tự) | 1. Nhập Mật khẩu 7 ký tự đủ điều kiện loại ký tự nhưng thiếu độ dài.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi độ dài mật khẩu tối thiểu 8 ký tự (biên dưới không hợp lệ) | Medium |
| TC_REG_015 | Đang ở form Đăng ký | Mật khẩu: "Abc@1234" (đúng 8 ký tự, đủ điều kiện) | 1. Nhập Mật khẩu 8 ký tự hợp lệ.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống chấp nhận, đăng ký thành công (biên dưới hợp lệ) | Medium |
| TC_REG_016 | Đang ở form Đăng ký | Mật khẩu: "Abcdefgh1234@5678900" (đúng 20 ký tự, đủ điều kiện) | 1. Nhập Mật khẩu 20 ký tự hợp lệ.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống chấp nhận, đăng ký thành công (biên trên hợp lệ) | Medium |
| TC_REG_017 | Đang ở form Đăng ký | Mật khẩu: "Abcdefgh1234@56789001" (21 ký tự) | 1. Nhập Mật khẩu 21 ký tự.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi vượt quá độ dài tối đa 20 ký tự (biên trên không hợp lệ) | Medium |
| TC_REG_018 | Đang ở form Đăng ký | Mật khẩu: "abc@1234" (thiếu chữ hoa) | 1. Nhập Mật khẩu không có chữ hoa.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi mật khẩu phải có ít nhất 1 chữ hoa | High |
| TC_REG_019 | Đang ở form Đăng ký | Mật khẩu: "ABC@1234" (thiếu chữ thường) | 1. Nhập Mật khẩu không có chữ thường.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi mật khẩu phải có ít nhất 1 chữ thường | High |
| TC_REG_020 | Đang ở form Đăng ký | Mật khẩu: "Abc@efgh" (thiếu số) | 1. Nhập Mật khẩu không có số.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi mật khẩu phải có ít nhất 1 chữ số | High |
| TC_REG_021 | Đang ở form Đăng ký | Mật khẩu: "Abc12345" (thiếu ký tự đặc biệt) | 1. Nhập Mật khẩu không có ký tự đặc biệt.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi mật khẩu phải có ít nhất 1 ký tự đặc biệt | High |

---

## F. Trường Nhập lại mật khẩu (EP + Error Guessing)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_022 | Đang ở form Đăng ký | Mật khẩu: "Abc@1234"; Nhập lại MK: "Abc@1235" (không khớp) | 1. Nhập Mật khẩu và Nhập lại Mật khẩu khác nhau.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi "Mật khẩu nhập lại không trùng khớp" | High |
| TC_REG_023 | Đang ở form Đăng ký | Mật khẩu: "Abc@1234"; Nhập lại MK: "" (bỏ trống) | 1. Nhập Mật khẩu hợp lệ, để trống Nhập lại mật khẩu.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi Nhập lại mật khẩu là bắt buộc | High |

---

## G. Nhóm tổ hợp Business Rule (Decision Table)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_024 | Trong hệ thống đã tồn tại email "vana.test01@gmail.com" và SĐT "0912345678" | Email: "vana.test01@gmail.com" (trùng); SĐT: "0912345678" (trùng) | 1. Nhập cả Email và SĐT đều đã tồn tại.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống báo lỗi "Email hoặc Số điện thoại đã được đăng ký" (không tạo tài khoản trùng) | Medium |

---

## H. State Transition Testing (Trạng thái kích hoạt tài khoản)

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_026 | Tài khoản vừa đăng ký thành công nhưng CHƯA click link kích hoạt trong email | Email: "vana.test01@gmail.com"; Mật khẩu: "Abc@1234" | 1. Truy cập trang Đăng nhập.<br>2. Nhập Email/SĐT và Mật khẩu của tài khoản chưa kích hoạt.<br>3. Nhấn "Đăng nhập" | Hệ thống báo lỗi "Tài khoản chưa kích hoạt, vui lòng kiểm tra email", không cho đăng nhập | High |

---

## I. Error Guessing / Security

| Test Case ID | Preconditions | Test Data | Test Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC_REG_027 | Đang ở form Đăng ký | Họ tên: `<script>alert('xss')</script>` hoặc `'; DROP TABLE users;--` | 1. Nhập chuỗi chứa mã script/SQL injection vào trường Họ tên.<br>2. Nhập hợp lệ các trường còn lại.<br>3. Nhấn "Đăng ký" | Hệ thống không thực thi mã độc, dữ liệu được escape/sanitize hoặc bị từ chối với thông báo lỗi hợp lý | Medium |

---

**Tổng số Test Case:** 26
**Ghi chú:**
- Các Test Case nhóm D (TC_REG_012a–e) có thể gộp thành 1 bảng Pairwise nếu muốn tối ưu số lượng case khi kết hợp với các điều kiện khác (VD: đầu số hợp lệ x độ dài đúng/sai).
- Priority được gán dựa trên mức độ ảnh hưởng nghiệp vụ: **High** = lỗi chặn luồng chính hoặc vi phạm bảo mật; **Medium** = lỗi biên/validate phụ; **Low** = lỗi trải nghiệm không ảnh hưởng nghiệp vụ cốt lõi.
