# EXECUTION LOG — UC16: View Interview Schedule
**Giai đoạn 2 — Thực thi test thủ công**

## Thông tin môi trường
| Mục | Giá trị |
|---|---|
| Hệ thống test | IMS (Interview Management System) |
| URL | (theo môi trường test thật đang dùng) |
| Account dùng để test | interviewer/IT (Role D), admin/IT (Role C) — đã dùng trong quá trình test |
| Ngày giờ chạy | 13/08/2026 |
| Người thực thi | Nguyễn Lê Ngọc Bảo |

| TC ID | Priority | Kết quả | Bug ID | Evidence | Ghi chú |
| --- | --- | --- | --- | --- | --- |
| TC16-10 | P0 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-11 | P0 | **Pass** |  | screenshot_16-11.png | Xác nhận qua ảnh role Interviewer: không thấy nút Add |
| TC16-01 | P1 | **Fail** | BUG-001 | screenshot_16-01.png | Thiếu cột "Interviewer"; cột "Result" không tách riêng mà gộp vào Status |
| TC16-02 | P1 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-03 | P1 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-04 | P1 | **Fail** | BUG-002 | screenshot_16-04.png | Message thực tế "No Interview Schedule has been found" không khớp ME008 |
| TC16-05 | P1 | **Blocked** | BUG-003 | screenshot_16-05.png | Chặn bởi BUG-003 — không có combo-box "Interviewer" |
| TC16-07 | P1 | **Blocked** | BUG-004 | screenshot_16-07.png | Chặn bởi BUG-004 — thiếu status "New"; 3 status còn lại (Invited/Interviewed/Cancelled) lọc **đúng** |
| TC16-09 | P1 | **Blocked** | — | — | Không có quyền/công cụ truy cập API để test |
| TC16-12 | P1 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-13 | P1 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-14 | P1 | Tạm dừng |  |  | PENDING — chờ BA xác nhận role "HR" (không tính là Pass) |
| TC16-15 | P1 | Tạm dừng |  |  | PENDING — như trên (không tính là Pass) |
| TC16-16 | P1 | **Fail** | BUG-005 | screenshot_16-16.png | Không role nào có icon View ở cột Action |
| TC16-18 | P1 | **Blocked** | — | — | Không có quyền chỉnh dữ liệu test (cần Result bất thường) |
| TC16-19 | P1 | **Fail** | BUG-006 | screenshot_16-19.png | Format hiện `DD-MM-YYYY` (gạch ngang) thay vì `DD/MM/YYYY` (gạch chéo) |
| TC16-20 | P1 | **Blocked** | — | — | Không có quyền tạo dữ liệu test sai định dạng |
| TC16-21 | P1 | **Blocked** | BUG-003 |  | Search + Status kết hợp hoạt động **đúng**; không test được đủ 3 filter do thiếu Interviewer |
| TC16-06 | P2 | **Blocked** | BUG-003 | screenshot_16-06.png | Không có cột/combo Interviewer |
| TC16-08 | P2 | **Pass** |  |  | Pass theo xác nhận gộp — chưa có evidence/log chi tiết riêng |
| TC16-17 | P2 | **Fail** | BUG-001 |  | Không có cột "Result" độc lập, chỉ thấy "Passed"/"Failed" gộp trong Status, không có N/A |
| TC16-22 | P2 | **Fail** | BUG-001 |  | Không có cột "Result" để kiểm tra giá trị khi Status = Cancelled |
| TC16-Job | — | Chưa chạy |  |  | BLOCKED sẵn — thiếu field spec (không tính là Pass) |

## Bảng tổng hợp
Tổng TC thiết kế : 22 (không tính TC16-Job)
Đã thực thi : 20
Pass : 7 (TC16-10, 11, 02, 03, 12, 13, 08)
Fail : 6 (TC16-01, 04, 16, 17, 19, 22)
Blocked : 7 (TC16-05, 06, 07, 09, 18, 20, 21)
Chưa chạy : 2 (TC16-14, TC16-15 — đang PENDING chờ BA)

Bug tìm được : 6 (Major: 4 [BUG-001, 003, 004, 005] | Minor: 2 [BUG-002, 006])

