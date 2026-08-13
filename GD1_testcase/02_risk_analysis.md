# 02. BUSINESS RULES & RISK ANALYSIS — UC16: View Interview Schedule

## PHẦN A — Business Rules

| ID | Loại | Mô tả | Trace về step/AC | Ghi chú Human Review |
|---|---|---|---|---|
| BRL-16-01 | Functional | Only user with Role A, Role B and Role C can edit the interview. Role D can view and submit result in schedule details. | Step 1, Step 2 | **Cần xác nhận**: REF 16 (Icon Submit result) trong UC gốc ghi "Not available for Manager, **HR** and Admin" — không khớp thuật ngữ "Recruiter" dùng ở Actor list. Chưa chốt "HR" = "Recruiter" hay là role thứ 5. Xem mục 01_ambiguity_analysis, mục C, #13. |
| BRL-16-02 | Validation | There're 4 statuses of the Interview Schedule: New, Invited, Interviewed, Cancelled. | Step 2 | Xác nhận đủ 4 trạng thái theo UC gốc (New/Invited/Interviewed/Cancelled). Hợp lệ. |
| BRL-16-03 | Constraint | The search box allows partial match on all columns in the result table. | Step 2 | Rule này **có tồn tại và hợp lệ**, khớp trực tiếp Screen Description REF 3. (Lưu ý: AI Self-Review ở node 7 từng khẳng định sai rằng BRL-16-03 không tồn tại — xem 05_self_review_log.md, mục "AI bịa/sai"). |
| BRL-16-04 | Dependency | The display of the "Add" icon is dependent on the user's role (only available for Role A, B, C). | Step 1 | Hợp lệ, khớp REF 7. Cần bổ sung rõ: Role D → **ẩn hoàn toàn**, không phải disable (xác nhận qua Human Review, xem 01_ambiguity_analysis mục B, #3). |
| BRL-16-05 | Hidden | The result column displays "N/A" if no result is submitted yet. | Step 2 | Hợp lệ. **Bổ sung mới** (Human Review phát hiện qua mock-up): khi Status = Cancelled, Result cũng hiển thị "N/A" — cần BA xác nhận chính thức hóa thành rule con của BRL-16-05. |

### Rule mới đề xuất bổ sung (từ Human Review)

| ID đề xuất | Loại | Mô tả | Nguồn phát hiện |
|---|---|---|---|
| BRL-16-06 (NEW) | Constraint | 3 điều kiện lọc (Search box, Interviewer, Status) khi dùng đồng thời áp dụng logic **AND**. | 01_ambiguity_analysis, mục B, #8 — Human Review |
| BRL-16-07 (NEW) | Hidden | Nếu Status = Cancelled thì Result luôn hiển thị "N/A" bất kể dữ liệu kết quả có tồn tại hay không. | 01_ambiguity_analysis, mục B, #6 — Human Review |
| BRL-16-08 (NEW, PENDING) | Missing spec | Cột "Job" trên bảng kết quả (thấy trên mock-up) hiện **chưa có business rule / field spec chính thức**. | 01_ambiguity_analysis, mục C, #12 — Human Review, cần BA bổ sung trước khi viết test case đầy đủ |

## PHẦN B — Đánh giá rủi ro

| ID | Likelihood | Impact | Risk Level | Lý do |
|---|---|---|---|---|
| BRL-16-01 | Medium | High | Medium | Có khả năng người dùng không được phép thực hiện hành động edit, có thể dẫn đến nhầm lẫn trong việc quản lý lịch phỏng vấn. **Risk tăng thêm** nếu "HR" thực sự là role riêng chưa được test. |
| BRL-16-02 | Low | High | Medium | Các trạng thái đã được định nghĩa rõ ràng, nhưng nếu không hiển thị đúng có thể gây nhầm lẫn cho người dùng. |
| BRL-16-03 | Medium | Medium | Medium | Nếu search box không hoạt động như mong đợi, người dùng có thể không tìm thấy thông tin cần thiết. |
| BRL-16-04 | High | Medium | High | Nếu không hiển thị đúng nút "Add", người dùng có thể không thực hiện được hành động cần thiết, ảnh hưởng đến quy trình làm việc. |
| BRL-16-05 | Medium | Medium | Medium | Nếu không hiển thị đúng kết quả cho các lịch phỏng vấn đã hủy, có thể gây nhầm lẫn cho người dùng về tình trạng phỏng vấn. |
| BRL-16-06 (NEW) | Medium | Medium | Medium | Nếu logic kết hợp filter sai (OR thay vì AND), kết quả search trả về sai, người dùng ra quyết định dựa trên dữ liệu sai. |
| BRL-16-08 (NEW, cột Job) | **High** | **High** | **High** | Field hoàn toàn chưa có spec — rủi ro cao nhất vì có thể hiển thị sai dữ liệu (nhầm Job của candidate/interviewer) mà **không có test case nào bắt lỗi** cho đến khi phát hiện thủ công. |

### Ghi chú
- Các điểm mơ hồ đã phát hiện trong tài liệu có thể làm tăng khả năng xảy ra lỗi, do đó đã được xem xét trong đánh giá rủi ro.
- **Cập nhật sau Human Review**: rủi ro cao nhất của UC16 không phải là các rule đã biết (BRL-16-01→05) mà là **field "Job" chưa được đặc tả** (BRL-16-08) — đây là rủi ro AI không phát hiện được ở bước phân tích ban đầu.