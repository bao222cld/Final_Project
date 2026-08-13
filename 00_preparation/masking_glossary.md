# Masking Glossary — UC16: View Interview Schedule

Bảng quy đổi dữ liệu nhạy cảm giữa tài liệu gốc và tài liệu đã làm sạch.

## 1. Ánh xạ vai trò (Role Mapping)

| Tên vai trò gốc | Mã hoá trong UC16_clean.md |
|---|---|
| Recruiter | Role A |
| Manager | Role B |
| Admin | Role C |
| Interviewer | Role D |

## 2. Các loại dữ liệu khác

| # | Loại dữ liệu | Giá trị gốc | Giá trị thay thế | Ghi chú |
|---|---|---|---|---|
| 1 | URL nội bộ | `https://ims.recruitment.com/candidate-list` | `https://app.example.local/candidate-list` | Domain nội bộ công ty → domain giả |
| 2 | Tên hệ thống | IMS (Interview Management System) | "IMS – HR System" (tên chung) | Giữ dạng mô tả chức năng, không gắn thương hiệu công ty cụ thể |
| 3 | Tên user đăng nhập trên mock-up | "hoannk" (góc phải màn hình) | Không nhúng vào tài liệu | Đã loại bỏ hoàn toàn khỏi UC16_clean.md thay vì chỉ đổi tên |
| 4 | Tên ứng viên/người phỏng vấn trong bảng dữ liệu mẫu | Nguyễn Anh Đức, Nguyễn Khắc Hoàn, Nguyễn Hiếu... | Không nhúng vào tài liệu | UC16_clean.md không chứa bảng dữ liệu mẫu (chỉ mô tả field/REF), nên không có tên người nào bị lộ |
| 5 | Ảnh mock-up (Pic.15, Pic.25) | Ảnh chụp màn hình gốc | Mô tả lại bằng bảng REF trong `UC16_clean.md`, không upload ảnh gốc lên AI | Đúng yêu cầu: KHÔNG upload ảnh, mô tả bằng bảng component |
| 6 | Tên các trường (field/biến nội bộ) | Không có tên biến nội bộ dạng code trong UC gốc | N/A | UC16 chỉ có label mô tả sẵn (Search box, Interviewer...), không có tên biến kiểu TextboxA/comboboxB cần mã hoá |
| 7 | Tên các button | Không có tên button custom trong UC gốc | N/A | Chỉ có "Search Button", "Add" — là label mô tả chức năng, không phải tên biến nội bộ |

## 3. Nguyên tắc sử dụng

- Khi AI (Agent/LLM) sinh Test Condition, Test Case dựa trên `UC16_clean.md`, kết quả sẽ dùng "Role A/B/C/D".
- **Trước khi đưa vào hệ thống thật / Jira thật**, phải dịch ngược theo bảng Mục 1 (VD: "Role A" → "Recruiter") khi viết bước thực hiện test case cụ thể, vì hệ thống thật dùng tên role thật.
- Nếu phát hiện thêm dữ liệu nhạy cảm khác trong quá trình làm GĐ2/GĐ3 (tài khoản test thật, token, session ID...), bổ sung vào bảng này và không đưa các giá trị đó vào bất kỳ input nào gửi cho AI.