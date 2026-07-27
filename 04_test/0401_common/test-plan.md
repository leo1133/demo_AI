---
title: Test Plan (tổng)
owner: QA Lead
status: draft
updated: 2026-06-03
---
# Test Plan — <TÊN DỰ ÁN>

## 1. Mục tiêu
Đảm bảo mọi yêu cầu chức năng (REQ) trong tài liệu SDR được kiểm chứng trước khi release, và sản phẩm đạt các yêu cầu phi chức năng (NFR).

## 2. Phạm vi theo module
| Module | Loại test áp dụng | Ưu tiên | Ghi chú |
|---|---|---|---|
| 050-number-pool | Functional, Integration | Trung bình | Quản lý kho số điện thoại 050, gán thiết bị |
| audit-log | Functional, Security | Cao | Ghi nhận nhật ký thao tác hệ thống phục vụ kiểm toán |
| department | Functional, UI | Cao | Quản lý sơ đồ phòng ban dưới dạng Accordion lồng nhau |
| jwt-response-update-token-revocation | Integration, Security | Cao | Xử lý token JWT và danh sách blacklist/thu hồi token |
| permission-matrix-access-control | Functional, Security | Cao | Kiểm soát phân quyền 5 nhóm và ràng buộc SoD |
| user-department-role-assignment | Functional | Cao | Gán phòng ban và vai trò phê duyệt cho tài khoản |
| warehouse | Functional | Trung bình | Quản lý danh mục kho master |

## 3. Loại test & định nghĩa
| Loại | Mục đích |
|---|---|
| Functional | Kiểm từng yêu cầu REQ hoạt động đúng mô tả |
| Negative | Kiểm hành vi với dữ liệu sai / trái phép |
| Regression | Đảm bảo thay đổi mới không phá tính năng cũ |
| Integration | Kiểm phối hợp giữa các module / API |
| Security | Kiểm phân quyền, xác thực, rò rỉ dữ liệu |
| UAT | Khách hàng nghiệm thu theo kịch bản nghiệp vụ |

## 4. Môi trường & dữ liệu test
| Môi trường | Mục đích | URL |
|---|---|---|
| dev | QA test hằng ngày | |
| staging | Regression + UAT | |
- Tài khoản test, dữ liệu mẫu: <mô tả / link>.

## 5. Tiêu chí Pass/Fail (Entry & Exit)
- **Entry:** Tài liệu SDR/DD đã ở trạng thái `approved`, build đã deploy lên môi trường test.
- **Exit / điều kiện release:** xem `release-checklist.md`.

## 6. Mức độ ưu tiên & severity bug
| Severity | Định nghĩa | Xử lý |
|---|---|---|
| Critical | Chặn nghiệp vụ chính, mất dữ liệu | Fix ngay, chặn release |
| Major | Sai chức năng quan trọng, có workaround | Fix trước release |
| Minor | Lỗi nhỏ, ít ảnh hưởng | Có thể release, fix sau |
| Trivial | Cosmetic (UI, chính tả) | Backlog |

## 7. Quy trình & báo cáo
Quy ước viết test case, vòng đời bug: xem `test-guide.md`.
