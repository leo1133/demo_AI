---
title: Hướng dẫn & quy ước Test
owner: QA Lead
updated: 2026-06-03
---
# Hướng dẫn viết & quản lý Test

## 1. Mỗi REQ phải có test case
Dự án chạy theo đặc tả SDR, nên mỗi yêu cầu chức năng `REQ-<MODULE>-xxx` cần ít nhất một test case phủ. QA dò danh sách yêu cầu chức năng trong `02-requirements/modules/<m>/sdr.md`, đảm bảo tất cả các REQ được phủ và file TC tồn tại.

## 2. Quy ước viết Test Case
- File: `test-cases/TC-<MODULE>-<số>-<mô-tả>.md`, mã `TC-<MODULE>-xxx`.
- Trong metadata, điền `covers:` = (các) mã REQ mà test case kiểm chứng → phục vụ truy vết.
- Mỗi TC nên có cả case **positive** (đúng) và **negative** (sai/biên).
- Mỗi bước ghi rõ: Hành động → Dữ liệu → Kết quả mong đợi.

## 3. Ma trận truy vết (Traceability)
Duy trì bảng REQ ↔ TC để biết độ phủ:

| REQ | Test case | Trạng thái |
|---|---|---|
| REQ-WAREHOUSE-001 | TC-WAREHOUSE-001 | Pass |
| REQ-WAREHOUSE-002 | TC-WAREHOUSE-002 | Chưa test |

> Có thể đặt bảng này trong `traceability-matrix.md` và cập nhật mỗi vòng test.

## 4. Vòng đời Bug
```
open → in-progress (dev fix) → fixed → retest → closed
                                   └── reopen (nếu fail) ──┘
```
- Báo bug: `bug-reports/BUG-<MODULE>-<số>.md`, điền `severity`, `related` (TC/REQ liên quan).
- Bug Critical/Major phải đóng trước khi release (xem `release-checklist.md`).

## 5. Khi nào chạy Regression
Trước mỗi release, hoặc khi có thay đổi ảnh hưởng module liên quan. Chọn lại các TC ưu tiên cao của module bị ảnh hưởng và các module phụ thuộc.
