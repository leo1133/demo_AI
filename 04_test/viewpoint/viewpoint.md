# Viewpoint-VN

| NO. | Control  | Test Item | Confirm Content |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | File upload | Kiểm tra UI của file upload  |  |  | Hiển thị UI giống với link design trên figma |  |
| 2 |  | Kiểm tra trạng thái button  |  | 1. Kiểm tra trạng thái của button "Upload" | 1. Button ở trạng thái enable và cho phép người dùng click  |  |
| 3 |  | Kiểm tra khi nhấn vào button  |  | 1. Thực hiện nhấn chọn vào button | 1. Hệ thống hiển thị cửa sổ cho phép người dùng upload file từ máy tính  |  |
| 4 |  | Kiểm tra file upload  | excel/docs/pdf/csv/.... | 1. Thực hiện chọn file {được phép upload} | Hệ thống cho phép upload  |  |
| 5 |  |  | rar/exe/..... | 1. Thực hiện chọn file {không được phép upload} | Hiển thị thông báo lỗi  |  |
| 6 |  | Kiểm tra dung lượng file upload | dung lượng < {dung lượng tối đa} | 1. Thực hiện upload file có dung lượng < {dung lượng tối đa} | Cho phép chọn  |  |
| 7 |  |  | dung lượng = {dung lượng tối đa} | 1. Thực hiện upload file có dung lượng = {dung lượng tối đa} | Cho phép chọn  |  |
| 8 |  |  | dung lượng = {dung lượng tối đa} | 1. Thực hiện upload file có dung lượng > {dung lượng tối đa} | Hiển thị thông báo lỗi  |  |
| 9 |  |  | file upload = 0kb | 1. Thực hiện upload file trống (0KB). | Hiển thị thông báo lỗi  |  |
| 10 |  | Kiểm tra khi thực hiện Upload đồng thời nhiều file |  | 1. Thực hiện chọn nhiều file để thực hiện upload  | Cho phép chọn / Hiển thị thông báo lỗi / Thay thế file cũ bằng file mới vừa chọn  |  |
| 11 |  | Kiểm tra khi upload < {max_file} |  |  | Cho phép upload thành công  |  |
| 12 |  | Kiểm tra khi upload = {max_file} |  |  | Cho phép upload thành công  |  |
| 13 |  | Kiểm tra khi upload > {max_file} |  |  | Hiển thị thông báo lỗi  |  |
| 14 |  | Kiểm tra thời gian upload file  |  |  | Thời gian upload file nằm trong thời gian chấp nhận  |  |
| 15 |  | Kiểm tra hiển thị sau khi file được upload thành công |  |  | Hiển thị tên file đã chọn trước đó  |  |
| 16 | Button | Kiểm tra khi di chuyển con trỏ chuột vào button  |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Màu sắc của button có sự thay đổi  |  |
| 17 |  | Kiểm tra trạng thái button  |  | 1. Kiểm tra trạng thái của button  | 1. Button ở trạng thái enable và cho phép người dùng click  |  |
| 18 |  | Kiểm tra kết quả sau khi nhấn vào button  |  | 1. Thực hiện nhấn chọn vào button | 1. Hệ thống hiển thị màn hình/popup/thực hiện thêm mới/chỉnh sửa/tìm kiếm/...... |  |
| 19 |  | Kiểm tra khi thực hiện double click  |  |  | 1. Hiển thị biểu tượng loading và không cho phép double click |  |
| 20 | Link/Hyperlink  | Kiểm tra style link |  |  | Link/Hyperlink hiển thị màu xanh, gạch chân  |  |
| 21 |  | Kiểm tra khi di chuyển con trỏ chuột vào link/hyperlink  |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Link đổi màu hoặc underline theo thiết kế |  |
| 22 |  | Kiểm tra responsive |  |  | Link không bị che, hiển thị rõ |  |
| 23 |  | Kiểm tra visited link |  | Click vào link rồi quay lại | Link đổi màu theo CSS visited |  |
| 24 |  | Kiểm tra khi click vào link/hyperlink  | Link Internal | Click vào link nội bộ | Mở đúng trang trong hệ thống | Là link trỏ đến trang trong cùng hệ thống / cùng domain.<br>Thường dùng để điều hướng menu, trang chi tiết, trang chức năng.<br> |
| 25 |  |  | Link External | Click vào link ngoài | Mở đúng website ngoài, thường trong tab mới | Là link trỏ đến website khác / domain khác.<br>Dẫn ra ngoài hệ thống<br>VD: https://myapp.com → https://facebook.com/myapp |
| 26 |  |  | Anchor | Click vào link #section | Trang scroll đến đúng vị trí section | Là link trỏ đến một vị trí cụ thể trong cùng trang (scroll đến section).<br>Không tải lại trang, chỉ scroll tới đúng vị trí.<br>Dùng dấu # để xác định id của element trong HTML.<br>Khi click link → trang cuộn tới đúng vị trí, không bị che bởi header, không nhảy sai section.<br>VD: https://myapp.com/help#faq → nhảy tới phần FAQ trong trang Help. |
| 27 |  |  | Email | Click vào link trong email | Dẫn đến đúng trang trong hệ thống | Là link có dạng mailto: để mở ứng dụng email của người dùng.<br>Khi click, mở app email mặc định (Outlook, Gmail client…) với sẵn địa chỉ email.<br>Click mở đúng app email, có điền sẵn địa chỉ/subject/cc/bcc như cấu hình. |
| 28 |  | Kiểm tra link khi bị disable |  | Thực hiện click vào link đang ở trạng thái disable  | Không thể click, không redirect |  |
| 29 | Image | Kiểm tra hiển thị image |  |  | - Image hiển thị rõ ràng, ảnh không bị vỡ kích thước<br>- Image được re-size vừa khuôn tròn/vuông |  |
| 30 |  | Kiểm tra khi thực hiện click vào image  |  |  | Cho phép người dùng xem image  |  |
| 31 |  | Kiểm tra khi thực hiện zoom in/zoom out image  |  |  | Ảnh có thể Zoom in / Zoom out  |  |
| 32 |  | Kiểm tra khi nhấn vào icon tải image  |  |  | 1. Hệ thống hiển thị cửa sổ cho phép người dùng chọn image  từ máy tính  |  |
| 33 |  | Kiểm tra image |  | 1. Thực hiện chọn image {được phép upload} | Cho phép chọn  |  |
| 34 |  |  |  | 1. Thực hiện chọn image {không được phép upload} | Hiển thị thông báo lỗi  |  |
| 35 |  | Kiểm tra dung lượng image  | dung lượng < {dung lượng tối đa} | 1. Thực hiện chọn image có dung lượng < {dung lượng tối đa} | Cho phép chọn  |  |
| 36 |  |  | dung lượng = {dung lượng tối đa} | 1. Thực hiện chọn image có dung lượng = {dung lượng tối đa} | Cho phép chọn  |  |
| 37 |  |  | dung lượng = {dung lượng tối đa} | 1. Thực hiện chọn image có dung lượng > {dung lượng tối đa} | Hiển thị thông báo lỗi  |  |
| 38 |  |  | file upload = 0kb | 1. Thực hiện upload file trống (0KB). | Hiển thị thông báo lỗi  |  |
| 39 |  | Kiểm tra khi thực hiện chọn đồng thời nhiều image  |  | 1. Thực hiện chọn nhiều image để thực hiện upload  | Cho phép chọn / Hiển thị thông báo lỗi / Thay thế file cũ bằng file mới vừa chọn  |  |
| 40 |  | Kiểm tra số lượng image tối đa được upload  | Kiểm tra khi upload < {max_image} |  | Cho phép chọn  |  |
| 41 |  |  | Kiểm tra khi upload = {max_image} |  | Cho phép chọn  |  |
| 42 |  |  | Kiểm tra khi upload > {max_image} |  | Hiển thị thông báo lỗi  |  |
| 43 |  | Kiểm tra thời gian upload image  |  |  | Thời gian upload file nằm trong thời gian chấp nhận  |  |
| 44 |  | Kiểm tra hiển thị sau khi image được upload thành công |  |  | Hiển thị tất cả các ảnh đã chọn  |  |
| 45 | Icon | Kiểm tra hiển thị icon  |  |  | Hiển thị đúng icon so với design <br>Căn chỉnh với text hoặc control liên quan. |  |
| 46 |  | Kiểm tra trạng thái của icon  |  |  | - Active/Inactive: icon đổi màu/opacity theo trạng thái.<br><br>- Visible/Invisible: có bị hiển thị khi không cần thiết.<br><br>- Hover/focus: có hiệu ứng (màu, viền, tooltip). |  |
| 47 |  | Kiểm tra khi click vào icon <br>( button icon ) |  |  | Trigger đúng action. |  |
| 48 |  | Kiểm tra khi click vào icon <br>( expand/collapse ) |  |  | Trạng thái icon thay đổi chính xác (ví dụ: mũi tên xuống → mũi tên lên). |  |
| 49 | Label | Kiểm tra hiển thị label  |  |  | - Label hiển thị đúng text theo đặc tả (font, size, color, style) design <br>- Không bị tràn, cắt chữ, xuống dòng sai.<br>- Căn chỉnh đúng với control liên quan (textbox, checkbox, radio, dropdown). |  |
| 50 |  | Kiểm tra nội dung trên label  |  |  | Hiển thị chính xác text, không lỗi  |  |
| 51 |  | Kiểm tra khi thay đổi ngôn ngữ <br>(multi-language) |  |  | - Không bị vỡ label<br>- Ngôn ngữ được thay đổi theo đúng language  |  |
| 52 |  | Kiểm tra khi di chuyển chuột vào label  |  |  | - Hiển thị con trỏ chuột <br>- Không thay đổi màu sắc của label  |  |
| 53 |  | Kiểm tra khi click vào label  |  |  | Không cho phép người dùng click vào label  |  |
| 54 | Tooltip  | Kiểm tra hiển thị chiều rộng và chiều cao của Tooltip  |  |  | Chiều rộng và chiều cao của Tooltip được căn chỉnh phù hợp  |  |
| 55 |  | Kiểm tra  văn bản hiển thị trên Tooltip |  |  | - Văn bản hiển thị trên Tooltip được căn chỉnh phù hợp <br>- Nội dung văn bản trên Tooltip được hiển thị đầy đủ, không cắt bớt  |  |
| 56 |  | Kiểm tra khi di chuyển chuột vào tooltip  |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Hiển thị nội dung tooltip  |  |
| 57 |  | Kiểm tra khi thực hiện chỉnh sửa nội dung văn bản trên tooltip  |  |  | Nội dung văn bản trên tooltip không được phép chỉnh sửa  |  |
| 58 | Table/Grid | Kiểm tra UI hiển thị khi chưa có dữ liệu |  |  | Hiển thị "There’s nothing here yet."  |  |
| 59 |  | Kiểm tra UI hiển thị khi có dữ liệu  |  |  | - Dữ liệu hiển thị đầy đủ, chính xác theo DB trả về<br>- Kích thước cột có thể cố định hay co giãn theo nội dung.<br>- Không bị vỡ layout khi dữ liệu nhiều hoặc tên cột dài.<br>- TH nhiều trường dữ liệu thì cho phép người dùng scroll ngang màn hình  |  |
| 60 |  | Kiểm tra khi hover chuột vào từng hàng  |  |  | Highlight khi hover. |  |
| 61 |  | Kiểm tra các trường hiển thị trên table/grid  |  |  | Hiển thị các field<br>+ STT <br>+ ...... |  |
| 62 |  | Kiểm tra dữ liệu tại table  | Kiểm tra dữ liệu tại trường STT  |  | - STT tự động tăng dần<br>- Hiển thị từ 1 - n  |  |
| 63 |  |  | Kiểm tra dữ liệu tại trường Name  |  | - Hiển thị name <br>- TH không có dữ liệu hiển thị "-"<br>- TH dữ liệu dài  |  |
| 64 |  |  | ..................<br>( Check tất cả các field trên table )  |  | - Hiển thị chính xác tên field <br>- TH không có dữ liệu hiển thị "-" hoặc "0" <br>- TH dữ liệu dài  |  |
| 65 |  | Kiểm tra khi thực hiện scroll  | Kiểm tra scroll khi table có dữ liệu lớn  |  | Cho phép người dùng scroll lên/xuống để xem toàn bộ dữ liệu của table  |  |
| 66 |  |  | Kiểm tra scroll khi table có nhiều trường dữ liệu  |  | Cho phép người dùng scroll ngang để xem toàn bộ dữ liệu các trường  |  |
| 67 | Pagination | Kiểm tra hiển thị UI Pagination |  |  | Các nút First, Previous, Next, Last hiển thị đúng |  |
| 68 |  | Kiểm tra hiển thị khi đang focus ở trang bất kì  |  |  | highlight trang đang được chọn  |  |
| 69 |  | Kiểm tra phân trang  | Số bản ghi trên trang = 0 |  | - Phân trang được ẩn  |  |
| 70 |  |  | Số bản ghi trên trang < 11 |  | - Dữ liệu được hiển thị trên 1 trang <br>- Phân trang được ẩn  |  |
| 71 |  |  | Số bản ghi trên trang = 10 |  | - Dữ liệu được hiển thị trên 1 trang <br>- Phân trang được ẩn  |  |
| 72 |  |  | Số bản ghi trên trang > 10  |  | - Dữ liệu được hiển thị > 1 trang <br>- Hiển thị phân trang <br>- Mỗi trang tối đa 10 bản ghi  |  |
| 73 |  | Kiểm tra icon <  | Kiểm tra khi hệ thống đang ở Trang đầu tiên |  | icon Previous bị disable. ( không hiển thị nút First/Previous ) |  |
| 74 |  |  | Kiểm tra khi hệ thống đang ở trang khác trang đầu tiên |  | icon Previous enable  |  |
| 75 |  |  | Kiểm tra khi nhấn icon <  |  | quay lại trang trước. |  |
| 76 |  | Kiểm tra icon < < | Kiểm tra khi hệ thống đang ở Trang đầu tiên |  | icon First bị disable. ( không hiển thị nút ) |  |
| 77 |  |  | Kiểm tra khi hệ thống đang ở trang khác trang đầu tiên |  | icon First enable  |  |
| 78 |  |  | Kiểm tra khi nhấn icon < < |  | quay lại trang đầu tiên |  |
| 79 |  | Kiểm tra icon > | Kiểm tra khi hệ thống đang ở Trang cuối cùng  |  | icon Last bị disable. ( không hiển thị nút  ) |  |
| 80 |  |  | Kiểm tra khi hệ thống đang ở trang khác trang cuối cùng  |  | icon Last enable  |  |
| 81 |  |  | Kiểm tra khi nhấn icon >  |  | chuyển tới trang tiếp theo  |  |
| 82 |  | Kiểm tra icon >> | Kiểm tra khi hệ thống đang ở Trang cuối cùng  |  | icon Last bị disable. ( không hiển thị nút  ) |  |
| 83 |  |  | Kiểm tra khi hệ thống đang ở trang khác trang cuối cùng  |  | icon Last enable  |  |
| 84 |  |  | Kiểm tra khi nhấn icon >>  |  | chuyển tới trang cuối. |  |
| 85 |  | Kiểm tra khi click trang bất kỳ |  |  | điều hướng đúng trang. |  |
| 86 | Treeview | Kiểm tra hiển thị cấu trúc  |  |  | Hiển thị đúng cấu trúc cha – con – cháu theo thiết kế. |  |
| 87 |  | Kiểm tra icon hiển thị  |  |  | Icon trạng thái hiển thị chính xác (mũi tên, +/–, caret…). |  |
| 88 |  | Kiểm tra dữ liệu hiển thị  |  |  | Dữ liệu hiển thị đầy đủ, đúng thứ tự |  |
| 89 |  | Kiểm tra khi click icon/label |  |  | - Hệ thống mở/đóng đúng theo thao tác<br>- Chỉ mở đúng nhánh được chọn, không ảnh hưởng nhánh khác |  |
| 90 |  | Kiểm tra khi thực hiện re-load trang |  |  | Giữ nguyên trạng thái expand/collapse |  |
| 91 |  | Kiểm tra khi thực hiện tick chọn  | Kiểm tra khi tick chọn nhiều node  |  | - Hệ thống cho phép chọn nhiều node<br>- Node đã chọn highlight chính xác |  |
| 92 |  |  | Kiểm tra khi tick chọn node cha |  | Tất cả các node con được chọn  |  |
| 93 |  |  | Kiểm tra khi chọn 1 node con |  | - Node đã chọn highlight chính xác<br>- Node cha không thực hiện highlight/ hiển thị dấu gạch ngang tại node cha |  |
| 94 |  |  | Kiểm tra khi thực hiện chọn tất cả các node con  |  | - Node đã chọn highlight chính xác<br>- Node cha được tick chọn và highlight node cha  |  |
| 95 |  | Kiểm tra khi thực hiện bỏ tick chọn  | Kiểm tra khi bỏ tick chọn node cha |  | Toàn bộ các node con được bỏ tick chọn  |  |
| 96 |  |  | Kiểm tra khi chọn 1 node con |  | - Node đã chọn được trả về trạng thái uncheck<br>- Node cha không thực hiện highlight/ hiển thị dấu gạch ngang tại node cha |  |
| 97 |  |  | Kiểm tra khi thực hiện bỏ chọn tất cả các node con  |  | - Node đã chọn được trả về trạng thái uncheck<br>- Node cha được bỏ tick chọn  |  |
| 98 | Accordion/Expandable panel | Kiểm tra hiển thị mặc định  |  |  | Mặc định hiển thị ở trạng thái Collapse |  |
| 99 |  | Kiểm tra hiển thị Accordion |  |  | hiển thị đủ title/header và nội dung bên trong |  |
| 100 |  | Kiểm tra khi di chuyển chuột vào icon |  |  | - Con trỏ chuột hiển thị hình bàn tay<br>- Màu sắc của icon có sự thay đổi  |  |
| 101 |  | Kiểm tra Icon mở/đóng | Kiểm tra trạng thái của Icon mở/đóng |  | - Enable cho phép người dùng click |  |
| 102 |  |  | Kiểm tra khi thực hiện click  |  | Trạng thái icon thay đổi đúng khi expand/collapse |  |
| 103 |  | Kiểm tra khi thực hiện click để mở panel  | Kiểm tra khi click vào 1 panel  |  | Cho phép người dùng click  |  |
| 104 |  |  | Kiểm tra khi click vào nhiều panel  |  | Cho phép người dùng mở nhiều panel / Đóng panel đã mở trước đó  |  |
| 105 | Textbox  | Kiểm tra hiển thị tại textbox | 1. Thực hiện kiểm tra hiển thị textbox |  | 1. <br>- Hiển thị [tên trường] dưới dạng textbox<br>- Cho phép nhập dữ liệu từ bàn phím  |  |
| 106 |  | Kiểm tra hiển thị khi nhập dữ liệu vào textbox | 1. Thực hiện nhập dữ liệu vào textbox  |  | 1. Dữ liệu hiển thị dưới dạng mã hoá ***** | Trường hợp viết testcase cho password  |
| 107 |  |  | 2. Click vào icon xem mật khẩu  |  | 2. Dữ liệu hiển thị mât khẩu dưới dạng string ‘Abcd’ |  |
| 108 |  |  | 3. Click lần 2 vào icon xem mật khẩu  |  | 3. Dữ liệu hiển thị mật khẩu về dạng mã hoá ***** |  |
| 109 |  | Kiểm tra khi thực hiện input data | 1. Thực hiện nhập ký tự là chữ <br>( chữ hoa, chữ thường ) |  | 1. Hệ thống cho phép nhập  | *NOTE: expect result cho phép nhập/không nhập tùy theo specs  |
| 110 |  |  | 1. Thực hiện nhập ký tự là số  |  | 1. <br>- Hệ thống không cho phép nhập <br>- Định dạng xxx.xxxx.xxxx / xxx,xxx,xxx | Định dạng check thêm với case là số tiền  |
| 111 |  |  | 1. Thực hiện nhập ký tự là ký tự đặc biệt <br>"@#$%^&*()" |  | 1. Hệ thống không cho phép nhập  |  |
| 112 |  |  | 1. Thực hiện nhập dữ liệu bắt đầu = 0  |  | 1. Hệ thống không cho phép nhập  | Trường hợp nhập số tiền sẽ check thêm case này  |
| 113 |  |  | 1. Thực hiện nhập data có chứa space ở đầu |  | 1. Hệ thống không cho phép nhập  |  |
| 114 |  |  | 1. Thực hiện nhập data có chứa space ở cuối |  | 1. Hệ thống không cho phép nhập  | 1. INPUT CONTROLS (Nhập dữ liệu) |
| 115 |  |  | 1. Thực hiện nhập data có chứa space ở giữa các ký tự |  | 1. Hệ thống không cho phép nhập  | Textbox (input text) |
| 116 |  |  | 1. Thực hiện nhập dữ liệu < minlenght |  | 1. Hệ thống cho phép nhập  | Password field |
| 117 |  |  | 1. Thực hiện nhập dữ liệu = minlenght |  | 1. Hệ thống cho phép nhập  | Textarea (multi-line) |
| 118 |  |  | 1. Thực hiện nhập minlength < data input < maxlength |  | 1. Hệ thống cho phép nhập  | Number input |
| 119 |  |  | 1. Thực hiện nhập dữ liệu = maxlenght |  | 1. Hệ thống cho phép nhập  | Email input |
| 120 |  |  | 1. Thực hiện nhập dữ liệu > maxlenght |  | 1. Hệ thống lấy tối đa x ký tự, từ ký tự thứ x+1 hệ thống chặn không cho nhập  | Phone input |
| 121 |  | Kiểm tra khi  khi paste data | 1. Thực hiện paste chuỗi ký tự < minlenght |  | 1. Hệ thống cho phép paste chuỗi ký tự  | Date picker / Time picker |
| 122 |  |  | 1. Thực hiện paste chuỗi ký tự = minlenght |  | 1. Hệ thống cho phép paste chuỗi ký tự  | File upload |
| 123 |  |  | 1. Thực hiện paste chuỗi ký tự  |  | 1. Hệ thống cho phép paste chuỗi ký tự  | Search box |
| 124 |  |  | 1. Thực hiện paste chuỗi dữ liệu min< data< max |  | 1. Hệ thống cho phép paste chuỗi ký tự  | Autocomplete / Suggestion input |
| 125 |  |  | 1. Thực hiện paste chuỗi dữ liệu = max |  | 1. Hệ thống cho phép paste chuỗi ký tự  |  |
| 126 |  |  | 1. Thực hiện paste chuỗi dữ liệu > max |  | 1. Hệ thống lấy tối đa x ký tự, từ ký tự thứ x+1 hệ thống chặn tự động loại bỏ  |  |
| 127 |  |  | 1. Thực hiện paste chuỗi ký tự gồm chữ, số, ký tự đặc biệt |  | 1. Hệ thống chỉ nhận các ký tự là số, các ký tự khác không cho phép paste vào textbox  | *NOTE: expect result tùy theo specs  |
| 128 |  | Kiểm tra khi nhập dữ liệu sau đó xoá toàn bộ data đã nhập | 1. Thực hiện nhập dữ liệu cho textbox <br>2. Xóa toàn bộ dữ liệu đã nhập  |  | 2. <br>- Clear toàn bộ dữ liệu ở textbox <br>- Báo lỗi/Không báo lỗi  |  |
|  | Droplist/Pulldown | Kiểm tra giá trị mặc định  | 1. Thực hiện kiểm tra giá trị mặc định tại droplist  |  |  |  |
|  |  | Kiểm tra list dữ liệu  |  |  |  |  |
|  |  | Kiểm tra khi chọn 1 dữ liệu  |  |  |  |  |
|  |  | Kiểm tra khi chọn nhiều dữ liệu |  |  |  |  |
| 129 | Checkbox  | Kiểm tra UI hiển thị  | 1. Thực hiện kiểm tra UI của dropdown  |  | 1. <br>- Hiển thị UI giống design <br>- Enable cho phép người dùng click  |  |
| 130 |  | Kiểm tra giá trị mặc định  | 1. Thực hiện kiểm tra giá trị mặc định tại dropdown  |  | 1. Hệ thống hiển thị giá trị mặc dịnh = All  |  |
| 131 |  | Kiểm tra list dữ liệu  | 1. Click mở dropdown <br>2. Thực hiện kiểm tra list dữ liệu trong dropdown  |  | 2. Hệ thống hiển thị list dữ liệu gồm <br>+ All <br>+ 1<br>+ 2<br>+ .....<br><br>// Hiển thị list dữ liệu được lấy từ màn hình (DB)  |  |
| 132 |  |  | 1. Click mở dropdown <br>2. Thực hiện scroll list dữ liệu  | Hệ thống có nhiều dữ liệu trong dropdown | 2. <br>- Scroll mượt<br>- Không mất dữ liệu |  |
| 133 |  |  | 1. Click mở dropdown <br>2. Thực hiện hover vào từng option |  | 2. Hệ thống thực hiện highlight đúng item đang hover  |  |
| 134 |  | Kiểm tra khi chọn data  | 1. Thực hiện click mở dropdown<br>2. Chọn 1 option |  | 2. <br>- Giá trị hiển thị đúng<br>- Dropdown đóng lại<br>- Value được lưu |  |
| 135 |  | Kiểm tra khi chọn lại option khác | 1. Thực hiện chọn option A thành công <br>2. Mở lại dropdown thực hiện chọn option B |  | 2. <br>- Value cập nhật đúng<br>- Không bị giữ giá trị cũ |  |
| 136 |  | Kiểm tra giá trị hiển thị sau khi re-load  | 1. Thực hiện chọn option bất kỳ thành công<br>2. Click F5 hoặc button re-load trên trình duyệt  |  | 2. Hệ thống hiển thị giá trị mặc định  |  |
| 137 |  | Kiểm tra khi click ra ngoài droplist | 1. Thực hiện click mở dropdown<br>2. Click ra ngoài vùng dropdown  |  | 2. Hệ thống thực hiện đóng Dropdown  |  |
| 138 |  | Kiểm tra khi điều hướng bằng bàn phím | 1. Thực hiện click mở dropdown<br>2. Dùng ↑ ↓  trên bàn phím |  | 2. <br>- Di chuyển được giữa các option<br>- Enter chọn giá trị |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
