# 03. TEST CONDITIONS — UC16: View Interview Schedule
**Trạng thái: Đã Human Review**

## Quy ước vai trò (bổ sung sau Human Review — cần BA xác nhận chính thức)
Bộ test case gốc dùng ký hiệu Role A/B/C/D nhưng UC không có bảng mapping tường minh. Đề xuất tạm dùng để review, **PENDING xác nhận BA**:

| Ký hiệu | Role thật (đề xuất) | Ghi chú |
|---|---|---|
| Role A | Recruiter | |
| Role B | Manager | |
| Role C | Admin | |
| Role D | Interviewer | |
| — | "HR" (REF 16) | **Chưa xác định** là Recruiter hay role riêng — xem 01_ambiguity_analysis #13 |

## Danh sách Test Conditions

| Cond ID | Mô tả điều kiện | Nguồn | Trạng thái review |
|---|---|---|---|
| Cond TC16-01 | Người dùng đã đăng nhập với Role A/B/C/D, hệ thống khả dụng, tồn tại dữ liệu lịch phỏng vấn | UC16 Basic Flow, Step 1-2 | ✅ Đã review |
| Cond TC16-02 | Hệ thống không khả dụng hoặc API tải dữ liệu trả lỗi | UC16 Post-condition (trường hợp lỗi) | ✅ Đã review |
| Cond TC16-03 | Tồn tại dữ liệu lịch phỏng vấn chứa từ khóa cần tìm ở ít nhất 1 cột hiển thị | BRL-16-03, REF 3 | ✅ Đã review — xác nhận search chỉ áp dụng trên cột hiển thị (xem 01_ambiguity_analysis #2) |
| Cond TC16-04 | Từ khóa nhập vào không khớp với bất kỳ dữ liệu nào trong bảng | REF 6, ME008 | ✅ Đã review |
| Cond TC16-05 | Tồn tại nhiều interviewer khác nhau trong dữ liệu | REF 4 | ✅ Đã review |
| Cond TC16-06 | Combo-box Interviewer để trống (giá trị blank) | REF 4 | ✅ Đã review |
| Cond TC16-07 | Dữ liệu tồn tại đủ 4 trạng thái: New, Invited, Interviewed, Cancelled | BRL-16-02 | ✅ Đã review |
| Cond TC16-08 | Chưa chọn Status (giá trị mặc định "All") | REF 5 | ✅ Đã review |
| Cond TC16-09 | Request gửi giá trị Status ngoài 4 giá trị hợp lệ (test qua API/devtools) | BRL-16-02 | ✅ Đã review |
| Cond TC16-10 | Đăng nhập với Role A/B/C | REF 7, BRL-16-04 | ✅ Đã review |
| Cond TC16-11 | Đăng nhập với Role D | REF 7, BRL-16-04 | ✅ Đã review |
| Cond TC16-12 | Đăng nhập với Role A/B/C | REF 15, BRL-16-01 | ✅ Đã review |
| Cond TC16-13 | Đăng nhập với Role D | REF 15, BRL-16-01 | ✅ Đã review |
| Cond TC16-14 | Đăng nhập với Role D | REF 16, BRL-16-01 | ⚠️ Review có điều kiện — phụ thuộc kết quả xác nhận "HR" (xem trên) |
| Cond TC16-15 | Đăng nhập với Role A/B/C | REF 16, BRL-16-01 | ⚠️ Review có điều kiện — như trên |
| Cond TC16-16 | Đăng nhập với role bất kỳ | REF 14 | ✅ Đã review |
| Cond TC16-17 | Dữ liệu có đủ 3 trường hợp: Passed, Failed, chưa có kết quả (N/A) | REF 11, BRL-16-05 | ✅ Đã review |
| Cond TC16-18 | Dữ liệu test có giá trị Result bất thường (khác Passed/Failed/N/A), chèn trực tiếp qua test data/DB | BRL-16-05 | ✅ Đã review |
| Cond TC16-19 | Dữ liệu có Schedule hợp lệ, 1 ngày | REF 12 | ✅ Đã review |
| Cond TC16-20 | Dữ liệu có Schedule sai định dạng (test data giả lập) | REF 12 | ✅ Đã review |
| **Cond TC16-21 (NEW)** | Kết hợp đồng thời Search box + Interviewer + Status trong 1 lần search | BRL-16-06 (NEW) — Human Review phát hiện gap coverage | 🆕 Bổ sung sau Human Review |
| **Cond TC16-22 (NEW)** | Dữ liệu lịch phỏng vấn có Status = Cancelled | BRL-16-07 (NEW) — Human Review | 🆕 Bổ sung sau Human Review |
| **Cond TC16-Job (OPEN)** | Dữ liệu có field "Job" hiển thị trên bảng kết quả | BRL-16-08 (NEW, PENDING) | ⛔ **Chưa thể viết test case đầy đủ** — thiếu field spec, cần BA bổ sung trước |