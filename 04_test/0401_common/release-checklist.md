---
title: Release Checklist (chung)
owner: QA
updated: 2026-06-03
---
# Checklist trước Release

## Độ phủ & kết quả test
- [ ] Mọi FR ưu tiên Cao đã có test case và **Pass** (đối chiếu `traceability-matrix.md`)
- [ ] Không còn bug **Critical/Major** ở trạng thái mở
- [ ] Regression đã chạy trên module thay đổi + module phụ thuộc
- [ ] Integration test giữa các module liên quan đã Pass

## Phi chức năng
- [ ] Kiểm bảo mật cơ bản (phân quyền, xác thực) — module nhạy cảm như auth
- [ ] Đáp ứng NFR (xem `03-technical/_common/non-functional-requirements.md`)

## Tài liệu & bàn giao
- [ ] SRS / API spec đã cập nhật theo thay đổi cuối
- [ ] Ghi release vào `CHANGELOG.md`
- [ ] (Nếu có) UAT đã được khách hàng nghiệm thu
