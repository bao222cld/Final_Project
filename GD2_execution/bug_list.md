# BUG LIST — UC16: View Interview Schedule
**Giai đoạn 2 — Thực thi test thủ công**

| Bug ID | TC ID | Severity | Mô tả lỗi | Kết quả thực tế | Kết quả mong đợi | Evidence | Trạng thái |
|---|---|---|---|---|---|---|---|
| BUG-001 | TC16-01, TC16-17, TC16-22 | Major | Bảng Interview Schedule thiếu cột "Interviewer" độc lập; cột "Result" không tồn tại riêng mà bị gộp chung vào Status (dạng "Interviewed - Pass/Fail"), không thể quan sát giá trị N/A hay Result khi Cancelled | Chỉ có cột: Interview Title, Candidate Name, Job Title, Schedule, Status(gộp Result), Action | Đủ các cột riêng biệt theo spec: Title, Candidate Name, Interviewer, Schedule, Result, Status, Job, Action | image_TC16-01.png | Open |
| BUG-002 | TC16-04 | Minor | Sai nội dung thông báo khi search không có kết quả | "No Interview Schedule has been found" | ME008: "No item matches with your search data. Please try again" | image_TC16-04.png | Open |
| BUG-003 | TC16-05, TC16-06, TC16-21 | Major | Thiếu combo-box "Interviewer" trên màn hình Interview Schedule | Chỉ có Search box + Status dropdown | Có đủ 3 filter: Search box, Interviewer, Status (theo REF 3-5) | image_TC16-05.png | Open |
| BUG-004 | TC16-07 | Major | Dropdown Status thiếu giá trị "New", có giá trị lạ "Open" thay thế | Status: Open, Invited, Interviewed, Cancelled | Status: New, Invited, Interviewed, Cancelled (theo BRL-16-02) | image_status_dropdown.png | Open |
| BUG-005 | TC16-16 | Major | Icon "View" không hiển thị ở cột Action cho bất kỳ role nào | Cột Action không có icon View | Icon View phải hiển thị cho mọi role (theo REF 14) | image_TC16-01.png | Open |
| BUG-006 | TC16-19 | Minor | Sai định dạng ký tự phân cách ngày tháng trong cột Schedule | `03-09-2027 18:00 - 22:15` (gạch ngang) | `03/09/2027 18:00 – 22:15` (gạch chéo, theo REF 12) | image_TC16-01.png | Open |

## Tổng hợp
Tổng bug tìm được: **6**
Critical: 0 | Major: **4** (BUG-001, 003, 004, 005) | Minor: **2** (BUG-002, 006)

> Ghi chú: severity ở trên là đề xuất, bạn nên rà lại và xác nhận (hoặc điều chỉnh) trước khi đưa vào Test Report ở Giai đoạn 4.