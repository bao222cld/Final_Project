# Anonymize Checklist — UC16

Checklist đối chiếu trực tiếp với bảng quy tắc "Thành phần trong UC / Xử lý dữ liệu nhạy cảm" của đề bài.

| # | Thành phần trong UC | Yêu cầu xử lý | Đã áp dụng trong UC16_clean.md? |
|---|---|---|---|
| 1 | Objective / Actor / Trigger / Pre-Post condition | Giữ nguyên | [x] Có |
| 2 | Activity Flow | Chuyển sang bảng: Step \| Actor \| Action \| Next | [x] Có — Basic Flow đã tách đủ 4 cột |
| 3 | Business Rules | Giữ nguyên (phần AI cần nhất) | [x] Có |
| 4 | Bảng Screen Components | Giữ nguyên | [x] Có |
| 5 | URL nội bộ | Thay bằng domain giả (app.example.local) | [x] Có |
| 6 | Ảnh mockup có logo | KHÔNG upload ảnh — mô tả bằng bảng component | [x] Có — không có ảnh nào được nhúng |
| 7 | Tên công ty / hệ thống | Đổi tên chung | [x] Có — "IMS – HR System" |
| 8 | Tên vai trò nội bộ | Khái quát hoá (Role A/B/C) | [x] Có — Recruiter/Manager/Admin/Interviewer → Role A/B/C/D (bảng ánh xạ tại masking_glossary.md) |
| 9 | Tên người / email / SĐT | Xoá hoặc thay User A/B | [x] Có — không nhúng bất kỳ tên người/username nào vào tài liệu |
| 10 | Tên các trường | TextboxA, comboboxB, linkuploadC | [x] N/A — UC gốc không có tên biến nội bộ dạng code |
| 11 | Tên các button | ButtonA, ButtonB | [x] N/A — UC gốc không có tên button custom dạng code |
| 12 | Nội dung label | Không cần liệt kê trong .md, tự viết trong test case | [x] Đã tuân thủ — không liệt kê label chi tiết ngoài mô tả REF |

## Checklist bổ sung trước khi upload cho AI 

- [x] Đã tách riêng 1 UC ra file .md (`UC16_clean.md`)
- [x] Đã xoá/thay TẤT CẢ URL nội bộ
- [x] Đã xoá/thay tên công ty, hệ thống
- [x] KHÔNG đưa ảnh mockup vào file
- [x] Đã chuyển hoá Activity Flow sang bảng đúng format Step\|Actor\|Action\|Next
- [x] Đã khái quát hoá vai trò hệ thống thành Role A/B/C/D
- [x] Không còn sót tên người / email / SĐT nào trong tài liệu
- [x] Bảng quy đổi (`masking_glossary.md`) giữ OFFLINE — không upload lên AI
- [x] Người ngoài đọc `UC16_clean.md` không đoán được công ty nào
- [x] Không có account/password/token nào bị lộ
- [x] Screenshot/log đính kèm (áp dụng ở GĐ2/GĐ3) — chưa phát sinh ở bước này

**Kết luận: UC16_clean.md ĐÃ TUÂN THỦ ĐẦY ĐỦ bảng quy tắc anonymize của đề bài, sẵn sàng đưa vào workflow GĐ1.**