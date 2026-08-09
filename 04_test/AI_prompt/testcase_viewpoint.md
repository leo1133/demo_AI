# Mục tiêu:

- Tự động sinh Test Case từ Requirement/UI Design bằng cách áp dụng bộ Viewpoint có sẵn.
- Flow: Requirement → AI xác định UI Component → Mapping Viewpoint → Generate Test Case → Review Coverage
- Điểm mạnh:
  - AI không tự sáng tạo Test Case từ đầu
  - AI phải sử dụng: Requirement + Viewpoint + Rule sinh Test Case => tạo bộ testcase hoàn chỉnh

# Prompt nên chia thành mấy loại?

Nên chia thành 4 prompt:

- prompt 1: Viewpoint Learning
- prompt 2: Requirement Analysis + Viewpoint Mapping
- prompt 3: Test Case Generation
- Prompt 4: Test Case Review + Coverage

## Prompt 1: Viewpoint Learning

#### Mục tiêu: Dạy AI hiểu:

- Viewpoint là gì
- Mỗi Viewpoint áp dụng cho Component nào
- Viewpoint gồm những Test Item nào
- Expected Result mẫu là gì
- Khi nào Viewpoint applicable
- Khi nào cần Requirement bổ sung

#### Cấu trúc prompt:

Bạn là Senior QA Engineer. Tôi cung cấp cho bạn một file Viewpoint dùng làm tiêu chuẩn kiểm thử UI.

Mục tiêu: Hãy đọc và xây dựng một Viewpoint Knowledge Base từ tài liệu được cung cấp. Viewpoint trong tài liệu được xem là QA Test Standard.

Nhiệm vụ: Với mỗi Viewpoint, bạn hãy

- Xác định UI Component.
- Xác định Test Item.
- Xác định Confirm Content / Expected Result.
- Xác định Test Data hoặc điều kiện đặc biệt nếu có.
- Xác định các parameter/placeholder như: `{max_file}, {dung lượng tối đa}, {minlength}, {maxlength}, ...`
- Xác định khi nào Viewpoint có thể áp dụng.
- Xác định khi nào cần Requirement bổ sung.

Quy tắc:

- Không tự sửa nội dung Viewpoint.
- Không tự thêm Business Rule.
- Không tự suy diễn giá trị của placeholder.
- Giữ nguyên ý nghĩa của Viewpoint.
- Nếu thông tin chưa đủ, đánh dấu `Need Clarification`.

Output
Viewpoint Knowledge Base

| Component | Test Item | Test Condition | Expected Result | Parameter |
| --------- | --------- | -------------- | --------------- | --------- |

Viewpoint Category
Phân loại Viewpoint theo:

- UI / Visual
- Functional
- Input Validation
- Boundary
- Error Handling
- Interaction
- Navigation
- Data Display
- Permission
- Performance
  Chú ý: Chỉ phân loại nếu nội dung Viewpoint thực sự hỗ trợ.

Parameter cần Requirement

| Parameter | Viewpoint | Requirement cần biết |
| --------- | --------- | -------------------- |

Ambiguous Viewpoint

- Liệt kê những Viewpoint chưa đủ thông tin để áp dụng chính xác.
- Không tự bổ sung thông tin.

#### Kết quả: AI sẽ biến file Markdown hiện tại thành một Knowledge Base có cấu trúc.

Button
├─ UI
├─ Enable / Disable
├─ Click
└─ Double Click

Textbox
├─ UI
├─ Input character
├─ Boundary
├─ Paste
└─ Clear data

Pagination
├─ UI
├─ Page boundary
├─ First
├─ Previous
├─ Next
├─ Last
└─ Page navigation

## Prompt 2: Requirement Analysis + Viewpoint Mapping

### Mục tiêu:

- AI phải nhận diện được từ Requirement các viewpoint nào sẽ được áp dụng.
- Mapping chính xác Requirement → UI Component → Applicable Viewpoint
