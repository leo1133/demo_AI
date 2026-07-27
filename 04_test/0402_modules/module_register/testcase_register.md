# Testcase: Đăng Ký Tài Khoản (UC-01)

> **Tác nhân:** Khách vãng lai (Guest)  
> **Tiền điều kiện chung:** Người dùng truy cập website và chưa đăng nhập.


---

## 🔵 UI/UX


### [UI/UX] Hiển thị Form đăng ký

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_001 | Kiểm tra hiển thị nút "Đăng ký" trên Header | Người dùng truy cập website, chưa đăng nhập. | 1. Quan sát khu vực Header của website. | 1. Hiển thị nút "Đăng ký" tại khu vực tài khoản trên Header. |
| TC_REG_002 | Kiểm tra hiển thị Form đăng ký khi click nút "Đăng ký" | Người dùng truy cập website, chưa đăng nhập. | 1. Click nút "Đăng ký" trên Header. | 1. Hệ thống hiển thị Form đăng ký gồm đầy đủ các trường: Họ và tên, Email, Số điện thoại, Mật khẩu, Nhập lại mật khẩu. |
| TC_REG_003 | Kiểm tra hiển thị trường Họ và tên dưới dạng textbox | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Quan sát trường Họ và tên.<br>2. Click vào textbox Họ và tên. | 1. Hiển thị trường Họ và tên dưới dạng textbox.<br>2.<br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím |
| TC_REG_004 | Kiểm tra hiển thị trường Email dưới dạng textbox | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Quan sát trường Email.<br>2. Click vào textbox Email. | 1. Hiển thị trường Email dưới dạng textbox.<br>2.<br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím |
| TC_REG_005 | Kiểm tra hiển thị trường Số điện thoại dưới dạng textbox | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Quan sát trường Số điện thoại.<br>2. Click vào textbox Số điện thoại. | 1. Hiển thị trường Số điện thoại dưới dạng textbox.<br>2.<br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím |
| TC_REG_006 | Kiểm tra hiển thị trường Mật khẩu dưới dạng textbox | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Quan sát trường Mật khẩu.<br>2. Click vào textbox Mật khẩu. | 1. Hiển thị trường Mật khẩu dưới dạng textbox.<br>2.<br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím |
| TC_REG_007 | Kiểm tra hiển thị trường Nhập lại mật khẩu dưới dạng textbox | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Quan sát trường Nhập lại mật khẩu.<br>2. Click vào textbox Nhập lại mật khẩu. | 1. Hiển thị trường Nhập lại mật khẩu dưới dạng textbox.<br>2.<br>- Con trỏ chuột hiển thị dạng Text (I-beam)<br>- Cho phép nhập dữ liệu từ bàn phím |
| TC_REG_008 | Kiểm tra hiển thị dữ liệu khi nhập vào trường Mật khẩu | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập dữ liệu vào textbox Mật khẩu.<br>2. Click vào icon xem mật khẩu.<br>3. Click lần 2 vào icon xem mật khẩu. | 1. Dữ liệu hiển thị dưới dạng mã hoá *****<br>2. Dữ liệu hiển thị mật khẩu dưới dạng string 'Abcd'<br>3. Dữ liệu hiển thị mật khẩu về dạng mã hoá ***** |
| TC_REG_009 | Kiểm tra hiển thị dữ liệu khi nhập vào trường Nhập lại mật khẩu | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập dữ liệu vào textbox Nhập lại mật khẩu.<br>2. Click vào icon xem mật khẩu.<br>3. Click lần 2 vào icon xem mật khẩu. | 1. Dữ liệu hiển thị dưới dạng mã hoá *****<br>2. Dữ liệu hiển thị mật khẩu dưới dạng string 'Abcd'<br>3. Dữ liệu hiển thị mật khẩu về dạng mã hoá ***** |

### [UI/UX] Trạng thái lỗi của textbox

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_010 | Kiểm tra ẩn thông báo lỗi khi nhập lại dữ liệu hợp lệ | Form đăng ký đang hiển thị và một trường đang báo lỗi đỏ. | 1. Thực hiện nhập dữ liệu hợp lệ vào textbox đang báo lỗi. | 1. Hệ thống tự động ẩn câu thông báo lỗi. |
| TC_REG_011 | Kiểm tra hiển thị lại thông báo lỗi khi nhập dữ liệu không hợp lệ | Form đăng ký đang hiển thị và một trường đang báo lỗi đỏ. | 1. Thực hiện nhập dữ liệu không hợp lệ vào textbox đang báo lỗi. | 1. Hệ thống báo msg lỗi tương đương. |

### [UI/UX] Xoá dữ liệu đã nhập

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_012 | Kiểm tra khi nhập dữ liệu sau đó xoá toàn bộ data đã nhập (áp dụng cho từng trường: Họ và tên, Email, Số điện thoại, Mật khẩu, Nhập lại mật khẩu) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập dữ liệu cho textbox.<br>2. Xóa toàn bộ dữ liệu đã nhập. | 2.<br>- Clear toàn bộ dữ liệu ở textbox<br>- Báo lỗi/Không báo lỗi |

---

## 🟡 Validation


### [Validation] Họ và tên

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_013 | Kiểm tra nhập ký tự là chữ (chữ hoa, chữ thường) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là chữ (chữ hoa, chữ thường) vào trường Họ và tên. | 1. Hệ thống cho phép nhập. |
| TC_REG_014 | Kiểm tra nhập ký tự là số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là số vào trường Họ và tên. | 1. Hệ thống cho phép nhập. |
| TC_REG_015 | Kiểm tra nhập ký tự đặc biệt "@#$%^&*()" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự đặc biệt "@#$%^&*()" vào trường Họ và tên. | 1. Hệ thống cho phép nhập. |
| TC_REG_016 | Kiểm tra nhập data có chứa space ở đầu | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở đầu vào trường Họ và tên. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống tự động trim space |
| TC_REG_017 | Kiểm tra nhập data có chứa space ở cuối | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở cuối vào trường Họ và tên. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống tự động trim space |
| TC_REG_018 | Kiểm tra nhập data có chứa space ở giữa các ký tự | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở giữa các ký tự vào trường Họ và tên. | 1. Hệ thống cho phép nhập. |
| TC_REG_019 | Kiểm tra paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt vào trường Họ và tên. | 1. Hệ thống cho phép paste chuỗi ký tự. |
| TC_REG_020 | Kiểm tra để trống trường Họ và tên khi submit | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Không nhập dữ liệu vào trường Họ và tên.<br>2. Điền đầy đủ các trường còn lại.<br>3. Nhấn "Đăng ký". | 3. Hệ thống hiển thị thông báo lỗi tại trường Họ và tên, không cho đăng ký thành công. |

### [Validation] Email

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_021 | Kiểm tra nhập ký tự là chữ (chữ hoa, chữ thường) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là chữ (chữ hoa, chữ thường) vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_022 | Kiểm tra nhập ký tự là số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là số vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_023 | Kiểm tra nhập ký tự đặc biệt "@#$%^&*()" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự đặc biệt "@#$%^&*()" vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_024 | Kiểm tra nhập data có chứa space ở đầu | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở đầu vào trường Email. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống tự động trim space |
| TC_REG_025 | Kiểm tra nhập data có chứa space ở cuối | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở cuối vào trường Email. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống tự động trim space |
| TC_REG_026 | Kiểm tra nhập data có chứa space ở giữa các ký tự | Test data: test @gmail.com | 1. Thực hiện nhập data có chứa space ở giữa các ký tự vào trường Email. | 1.<br>- Hệ thống cho phép nhập<br>- Báo lỗi "Email không hợp lệ" |
| TC_REG_027 | Kiểm tra nhập email không có @ | Test data: testgmail.com | 1. Thực hiện nhập email không có @ vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_028 | Kiểm tra nhập email thiếu domain | Test data: test@ | 1. Thực hiện nhập email thiếu domain vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_029 | Kiểm tra nhập email thiếu username | Test data: @gmail.com | 1. Thực hiện nhập email thiếu username vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_030 | Kiểm tra nhập email có chứa nhiều @ | Test data: test@@gmail.com | 1. Thực hiện nhập email có chứa nhiều @ vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_031 | Kiểm tra nhập email có domain không hợp lệ | Test data: test@gmail | 1. Thực hiện nhập email có domain không hợp lệ vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_032 | Kiểm tra nhập email có sub domain | Test data: test@mail.google.com | 1. Thực hiện nhập email có sub domain vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_033 | Kiểm tra nhập email có chứa dấu chấm | Test data: first.last@gmail.com | 1. Thực hiện nhập email có chứa dấu chấm vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_034 | Kiểm tra nhập email có chứa dấu cộng (+) | Test data: test+1@gmail.com | 1. Thực hiện nhập email có chứa dấu cộng (+) vào trường Email. | 1. Hệ thống cho phép nhập. |
| TC_REG_035 | Kiểm tra nhập email có chứa ký tự không hợp lệ | Test data: test<>@gmail.com | 1. Thực hiện nhập email có chứa ký tự không hợp lệ vào trường Email. | 1. Báo lỗi "Email không hợp lệ" |
| TC_REG_036 | Kiểm tra paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt vào trường Email. | 1. Hệ thống cho phép paste chuỗi ký tự. |
| TC_REG_037 | Kiểm tra để trống trường Email khi submit | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Không nhập dữ liệu vào trường Email.<br>2. Điền đầy đủ các trường còn lại.<br>3. Nhấn "Đăng ký". | 3. Hệ thống hiển thị thông báo lỗi tại trường Email, không cho đăng ký thành công. |

### [Validation] Số điện thoại

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_038 | Kiểm tra nhập ký tự là số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là số vào trường Số điện thoại. | 1. Hệ thống cho phép nhập. |
| TC_REG_039 | Kiểm tra nhập ký tự là chữ (chữ hoa, chữ thường) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là chữ (chữ hoa, chữ thường) vào trường Số điện thoại. | 1. Hệ thống chỉ nhận các ký tự là số, các ký tự khác không cho phép nhập vào textbox. |
| TC_REG_040 | Kiểm tra nhập ký tự đặc biệt "@#$%^&*()" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự đặc biệt "@#$%^&*()" vào trường Số điện thoại. | 1. Hệ thống chỉ nhận các ký tự là số, các ký tự khác không cho phép nhập vào textbox. |
| TC_REG_041 | Kiểm tra paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt vào trường Số điện thoại. | 1. Hệ thống chỉ nhận các ký tự là số, các ký tự khác không cho phép paste vào textbox. |
| TC_REG_042 | Kiểm tra để trống trường Số điện thoại khi submit | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Không nhập dữ liệu vào trường Số điện thoại.<br>2. Điền đầy đủ các trường còn lại.<br>3. Nhấn "Đăng ký". | 3. Hệ thống hiển thị thông báo lỗi tại trường Số điện thoại, không cho đăng ký thành công. |

### [Validation] Mật khẩu

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_043 | Kiểm tra nhập ký tự là chữ (chữ hoa, chữ thường) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là chữ (chữ hoa, chữ thường) vào trường Mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_044 | Kiểm tra nhập ký tự là số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là số vào trường Mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_045 | Kiểm tra nhập ký tự đặc biệt "@#$%^&*()" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự đặc biệt "@#$%^&*()" vào trường Mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_046 | Kiểm tra nhập data có chứa space ở đầu | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở đầu vào trường Mật khẩu. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống không trim space, coi space như 1 ký tự và được mã hóa |
| TC_REG_047 | Kiểm tra nhập data có chứa space ở cuối | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở cuối vào trường Mật khẩu. | 1.<br>- Hệ thống cho phép nhập<br>- Hệ thống không trim space, coi space như 1 ký tự và được mã hóa |
| TC_REG_048 | Kiểm tra nhập data có chứa space ở giữa các ký tự | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập data có chứa space ở giữa các ký tự vào trường Mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_049 | Kiểm tra paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt vào trường Mật khẩu. | 1. Hệ thống cho phép paste chuỗi ký tự. |
| TC_REG_050 | Kiểm tra nhập mật khẩu < 8 ký tự (nhỏ hơn minlength theo BR) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập mật khẩu có độ dài nhỏ hơn 8 ký tự vào trường Mật khẩu.<br>2. Nhấn "Đăng ký". | 2. Hệ thống hiển thị thông báo lỗi, không cho đăng ký thành công (theo BR: Mật khẩu tối thiểu 8 ký tự). |
| TC_REG_051 | Kiểm tra nhập mật khẩu = 8 ký tự, có đủ 1 hoa, 1 thường, 1 số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập mật khẩu đúng 8 ký tự, có ít nhất 1 chữ hoa, 1 chữ thường, 1 chữ số vào trường Mật khẩu. | 1. Hệ thống cho phép nhập (thỏa BR: Mật khẩu tối thiểu 8 ký tự, gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 chữ số). |
| TC_REG_052 | Kiểm tra nhập mật khẩu không có chữ hoa | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập mật khẩu ≥ 8 ký tự nhưng không có chữ hoa vào trường Mật khẩu.<br>2. Nhấn "Đăng ký". | 2. Hệ thống hiển thị thông báo lỗi, không cho đăng ký thành công (theo BR: Mật khẩu phải có ít nhất 1 chữ hoa). |
| TC_REG_053 | Kiểm tra nhập mật khẩu không có chữ thường | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập mật khẩu ≥ 8 ký tự nhưng không có chữ thường vào trường Mật khẩu.<br>2. Nhấn "Đăng ký". | 2. Hệ thống hiển thị thông báo lỗi, không cho đăng ký thành công (theo BR: Mật khẩu phải có ít nhất 1 chữ thường). |
| TC_REG_054 | Kiểm tra nhập mật khẩu không có chữ số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập mật khẩu ≥ 8 ký tự nhưng không có chữ số vào trường Mật khẩu.<br>2. Nhấn "Đăng ký". | 2. Hệ thống hiển thị thông báo lỗi, không cho đăng ký thành công (theo BR: Mật khẩu phải có ít nhất 1 chữ số). |
| TC_REG_055 | Kiểm tra để trống trường Mật khẩu khi submit | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Không nhập dữ liệu vào trường Mật khẩu.<br>2. Điền đầy đủ các trường còn lại.<br>3. Nhấn "Đăng ký". | 3. Hệ thống hiển thị thông báo lỗi tại trường Mật khẩu, không cho đăng ký thành công. |

### [Validation] Nhập lại mật khẩu

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_056 | Kiểm tra nhập ký tự là chữ (chữ hoa, chữ thường) | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là chữ (chữ hoa, chữ thường) vào trường Nhập lại mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_057 | Kiểm tra nhập ký tự là số | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự là số vào trường Nhập lại mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_058 | Kiểm tra nhập ký tự đặc biệt "@#$%^&*()" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện nhập ký tự đặc biệt "@#$%^&*()" vào trường Nhập lại mật khẩu. | 1. Hệ thống cho phép nhập. |
| TC_REG_059 | Kiểm tra paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt vào trường Nhập lại mật khẩu. | 1. Hệ thống cho phép paste chuỗi ký tự. |
| TC_REG_060 | Kiểm tra để trống trường Nhập lại mật khẩu khi submit | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Không nhập dữ liệu vào trường Nhập lại mật khẩu.<br>2. Điền đầy đủ các trường còn lại.<br>3. Nhấn "Đăng ký". | 3. Hệ thống hiển thị thông báo lỗi tại trường Nhập lại mật khẩu, không cho đăng ký thành công. |
| TC_REG_061 | Kiểm tra Mật khẩu và Nhập lại mật khẩu không khớp | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Nhập Mật khẩu hợp lệ.<br>2. Nhập Nhập lại mật khẩu khác với Mật khẩu đã nhập.<br>3. Nhấn "Đăng ký". | 3. Hệ thống báo lỗi tại trường Nhập lại mật khẩu: "Mật khẩu nhập lại không trùng khớp", không cho đăng ký thành công. |
| TC_REG_062 | Kiểm tra Mật khẩu và Nhập lại mật khẩu khớp nhau | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. | 1. Nhập Mật khẩu hợp lệ.<br>2. Nhập Nhập lại mật khẩu giống với Mật khẩu đã nhập. | 2. Hệ thống không hiển thị lỗi tại trường Nhập lại mật khẩu. |

---

## 🟢 Business


### [Business] Đăng ký thành công

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_063 | Kiểm tra đăng ký thành công với thông tin hợp lệ và Email chưa tồn tại trong hệ thống | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. Email sử dụng để đăng ký chưa tồn tại trong hệ thống. | 1. Điền đầy đủ và hợp lệ các trường: Họ và tên, Email, Số điện thoại, Mật khẩu, Nhập lại mật khẩu (Mật khẩu khớp Nhập lại mật khẩu).<br>2. Nhấn "Đăng ký". | 2.<br>- Hệ thống kiểm tra hợp lệ dữ liệu và kiểm tra Email trùng lặp, không phát hiện lỗi.<br>- Hệ thống khởi tạo tài khoản mới với trạng thái "Hoạt động", lưu vào DB.<br>- Hệ thống hiển thị thông báo đăng ký thành công.<br>- Hệ thống tự động đăng nhập cho người dùng. |

### [Business] Đăng ký thất bại - Email trùng lặp

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_064 | Kiểm tra đăng ký với Email đã tồn tại trong hệ thống | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. Email sử dụng để đăng ký đã tồn tại trong hệ thống. | 1. Điền đầy đủ và hợp lệ các trường trong Form, trong đó Email là Email đã được đăng ký trước đó.<br>2. Nhấn "Đăng ký". | 2. Hệ thống hiển thị thông báo lỗi "Email này đã được đăng ký, vui lòng sử dụng email khác", không tạo tài khoản mới. |

### [Business] Kiểm tra trạng thái tài khoản sau đăng ký

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_065 | Kiểm tra tài khoản mới khởi tạo có trạng thái "Hoạt động" | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. Email sử dụng để đăng ký chưa tồn tại trong hệ thống. | 1. Điền đầy đủ và hợp lệ thông tin đăng ký.<br>2. Nhấn "Đăng ký".<br>3. Kiểm tra trạng thái tài khoản vừa khởi tạo trong DB. | 3. Tài khoản mới được lưu vào DB với trạng thái "Hoạt động". |

### [Business] Kiểm tra tự động đăng nhập sau đăng ký

| ID | Sub Testcase Name | Pre-condition | Steps | Expected Result |
|---|---|---|---|---|
| TC_REG_066 | Kiểm tra hệ thống tự động đăng nhập ngay sau khi đăng ký thành công | Người dùng truy cập website, chưa đăng nhập, đã click "Đăng ký" trên Header và Form đăng ký đang hiển thị. Email sử dụng để đăng ký chưa tồn tại trong hệ thống. | 1. Điền đầy đủ và hợp lệ thông tin đăng ký.<br>2. Nhấn "Đăng ký". | 2. Sau khi đăng ký thành công, hệ thống tự động đưa người dùng vào trạng thái đã đăng nhập (không yêu cầu đăng nhập lại). |