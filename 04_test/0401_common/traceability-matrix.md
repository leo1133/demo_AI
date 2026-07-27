---
title: Traceability Matrix (FR ↔ Test)
owner: QA
updated: 2026-07-16
---
# Ma trận truy vết — FR ↔ Test Case
> Cập nhật mỗi vòng test để theo dõi độ phủ. Trạng thái: Chưa test | Pass | Fail | Blocked.

## Module auth
| FR | Mô tả ngắn | Test case | Trạng thái |
|---|---|---|---|
| FR-AUTH-001 | Đăng nhập | TC-AUTH-001 | Chưa test |
| FR-AUTH-002 | Khoá sau 5 lần sai | TC-AUTH-002 | Chưa test |
| FR-AUTH-003 | Quên mật khẩu | TC-AUTH-003 | Chưa test |
| FR-AUTH-004 | Đăng xuất | TC-AUTH-004 | Chưa test |
| FR-AUTH-005 | Phân quyền | TC-AUTH-005 | Chưa test |

## Module order
| FR | Mô tả ngắn | Test case | Trạng thái |
|---|---|---|---|
| FR-ORDER-001 | Tạo đơn | TC-ORDER-001 | Chưa test |
| FR-ORDER-002 | Tính tổng tiền | TC-ORDER-002 | Chưa test |
| FR-ORDER-003 | Huỷ đơn | TC-ORDER-003 | Chưa test |
| FR-ORDER-004 | Lịch sử đơn | TC-ORDER-004 | Chưa test |
| FR-ORDER-005 | Cập nhật trạng thái | TC-ORDER-005 | Chưa test |

## Module jwt-response-update-token-revocation
| FR | Mô tả ngắn | Test case | Trạng thái |
|---|---|---|---|
| REQ_JWT_001 | Mở rộng JWT Response payload (phòng ban, vai trò) | KS_JWT_REVOKE_TC_001, KS_JWT_REVOKE_TC_002, KS_JWT_REVOKE_TC_003, KS_JWT_REVOKE_TC_004, KS_JWT_REVOKE_TC_005, KS_JWT_REVOKE_TC_006 | Chưa test |
| REQ_JWT_002 | Tự động cập nhật permissions_updated_at và Redis | KS_JWT_REVOKE_TC_007, KS_JWT_REVOKE_TC_008, KS_JWT_REVOKE_TC_009, KS_JWT_REVOKE_TC_010, KS_JWT_REVOKE_TC_011, KS_JWT_REVOKE_TC_012 | Chưa test |
| REQ_JWT_003 | API Thu hồi Token thủ công (Manual Revocation) | KS_JWT_REVOKE_TC_013, KS_JWT_REVOKE_TC_014, KS_JWT_REVOKE_TC_015, KS_JWT_REVOKE_TC_016, KS_JWT_REVOKE_TC_017, KS_JWT_REVOKE_TC_018, KS_JWT_REVOKE_TC_019, KS_JWT_REVOKE_TC_020 | Chưa test |
| REQ_JWT_004 | Xác thực Middleware (iat vs updated_at, Fallback, Fail-Open, Blacklist) | KS_JWT_REVOKE_TC_021, KS_JWT_REVOKE_TC_022, KS_JWT_REVOKE_TC_023, KS_JWT_REVOKE_TC_024, KS_JWT_REVOKE_TC_025, KS_JWT_REVOKE_TC_026, KS_JWT_REVOKE_TC_027, KS_JWT_REVOKE_TC_028, KS_JWT_REVOKE_TC_029 | Chưa test |
| - | Audit Logs & An toàn thông tin | KS_JWT_REVOKE_TC_030, KS_JWT_REVOKE_TC_031, KS_JWT_REVOKE_TC_032 | Chưa test |

## Module permission-matrix-access-control
| FR | Mô tả ngắn | Test case | Trạng thái |
|---|---|---|---|
| REQ_AC_001 | Thực thi Ma trận phân quyền 5 roles (Sidebar, nút bấm UI, check API) | KS_AC_TC_006 ~ KS_AC_TC_010, KS_AC_TC_017 | Chưa test |
| REQ_AC_001 | Giao diện màn hình 403 Forbidden & Nút về Dashboard | KS_AC_TC_011, KS_AC_TC_012 | Pass |
| REQ_AC_002 / VAL-AC-02 | Phân tách trách nhiệm SoD (Chống người tạo tự phê duyệt) | KS_AC_TC_013, KS_AC_TC_014, KS_AC_TC_015 | Chưa test |
| REQ_AC_003 | Token JWT chứa claims vai trò, phòng ban đầy đủ | KS_AC_TC_001, KS_AC_TC_002 | Chưa test |
| REQ_AC_004 | Cơ chế thu hồi Token tức thì khi đổi quyền/vô hiệu hóa | KS_AC_TC_005 | Chưa test |
| VAL-AC-01 | Kiểm tra tính hợp lệ JWT (Signature, exp) | KS_AC_TC_003, KS_AC_TC_004 | Chưa test |
| VAL-AC-03 | Phòng ban chỉ có tối đa 1 người phê duyệt (APPROVER) | KS_AC_TC_016 | Chưa test |
