# Báo cáo Requirement Review Lần 2 (Góc độ QA)
**Tài liệu tham chiếu:** SRS v1.1
**Vai trò:** Senior QA

Dựa trên bản SRS v1.1 (đã giải quyết các câu hỏi ở vòng 1), dưới góc độ QA phân tích sâu hơn vào các luồng ngoại lệ (Edge cases) và kịch bản thực tế, tôi tiếp tục phát hiện một số điểm chưa rõ ràng cần BA làm rõ:

## 1. UC01, UC02, UC03: Đăng ký, Đăng nhập, Quên mật khẩu
- **[Medium] Validation chưa rõ:** Trường "Họ và tên" cấm ký tự đặc biệt, vậy khoảng trắng (Space) giữa các từ có được phép không? Nếu người dùng lỡ copy/paste có chứa khoảng trắng ở đầu/cuối chuỗi thì hệ thống có tự động cắt bỏ (auto-trim) không hay báo lỗi?
- **[High] Business Rule chưa rõ:** Khi tài khoản bị khóa 30 phút do nhập sai mật khẩu 5 lần. Trong thời gian khóa này, nếu người dùng sử dụng tính năng "Quên mật khẩu" và đặt lại mật khẩu thành công, tài khoản có được mở khóa (Unlock) ngay lập tức để đăng nhập không?
- **[High] Exception/Security chưa rõ:** Chức năng "Gửi lại" mã OTP/Link (có hiệu lực 15 phút) có cơ chế chống Spam không? (Ví dụ: Người dùng phải chờ 60 giây mới được nhấn gửi lại, hoặc giới hạn tối đa 5 lần gửi lại trong 1 ngày?).

## 2. UC04: Trang chủ (Home Page)
- **[Low] Exception chưa rõ:** Nếu trong vòng 7 ngày qua hệ thống không có bất kỳ sản phẩm nào được đăng tải mới, thì khối "Sản phẩm mới nhất" sẽ được hiển thị như thế nào? (Bị ẩn đi hoàn toàn, hiển thị Empty state, hay sẽ nới lỏng điều kiện để lấy các sản phẩm cũ hơn?).

## 3. UC05: Giỏ hàng (Shopping Cart)
- **[High] Boundary chưa rõ:** Đã có giới hạn tối đa 50 loại SKU khác nhau/giỏ. Vậy với mỗi loại SKU, khách hàng có bị giới hạn số lượng mua tối đa (Max quantity per item) không? (Ví dụ: Ngăn chặn mua sỉ quá 100 sản phẩm/SKU dù tồn kho còn rất lớn).
- **[High] Business Rule/Exception chưa rõ (Gộp giỏ hàng):** Khi khách vãng lai đăng nhập và xảy ra thao tác gộp (merge) giỏ hàng: Nếu sản phẩm A đang có số lượng là 2 ở giỏ tạm (Guest), và cũng có sẵn số lượng là 3 ở giỏ thành viên (Customer), hệ thống sẽ cộng dồn thành 5 hay lấy số lượng lớn nhất? Nếu cộng dồn thành 5 nhưng tồn kho thực tế chỉ còn 4, hệ thống sẽ xử lý và báo lỗi ra sao ngay sau khi đăng nhập?

## 4. UC06: Thanh toán (Checkout)
- **[High] Business Rule/Exception chưa rõ:** Đối với phương thức thanh toán COD (Thanh toán khi nhận hàng), hệ thống có giới hạn giá trị đơn hàng tối đa không? (Ví dụ: Đơn hàng > 20 triệu VNĐ bắt buộc thanh toán trực tuyến để chống rủi ro khách "bom" hàng giá trị cao?).
- **[Medium] Business Rule chưa rõ:** Khách hàng có được phép chọn và áp dụng nhiều Mã giảm giá (Coupon) cùng lúc cho 1 đơn hàng không? (Ví dụ: Vừa áp dụng mã Freeship vừa áp dụng mã giảm giá 10%).
- **[Medium] Exception/Business Flow chưa rõ:** Tại bước bấm nút "Xác nhận đặt hàng", hệ thống sẽ trừ tồn kho thực tế ngay lúc tạo đơn (kể cả chọn thanh toán Online nhưng chưa quét mã), hay phải chờ thanh toán Online thành công mới trừ kho? (Điều này ảnh hưởng rất lớn đến việc giam hàng / overselling).

## 5. UC07: Lịch sử mua hàng (Purchase History)
- **[High] Business Flow chưa rõ (Refund):** Nếu khách hàng tự nhấn "Hủy đơn hàng" đối với đơn hàng ĐÃ thanh toán thành công qua Thẻ/Ví điện tử, luồng hoàn tiền (Refund) sẽ diễn ra tự động thông qua cổng thanh toán hay cần có sự can thiệp thủ công từ Kế toán? (Cần thông báo gì cho khách hàng lúc hủy).
- **[Medium] Business Rule chưa rõ:** Sau khi đơn hàng bị hủy thành công (bởi khách hoặc Admin), số lượng sản phẩm của đơn hàng đó có tự động được cộng ngược lại (Restock) vào kho hàng hiện tại để người khác có thể mua không?
