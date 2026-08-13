# TEST SUMMARY REPORT - UC16 (View Interview Schedule)

## 1. SỐ LIỆU THỰC THI

| Chỉ số | Giá trị |
|---|---|
| Tổng test case | 22 |
| Đã thực thi | 20 |
| Pass | 7 |
| Fail | 6 |
| Blocked | 7 |

## 2. TÌNH TRẠNG BUG

| Mức độ | Số lượng còn mở |
|---|---|
| Critical | 0 |
| Major | 4 |
| Minor | 2 |

## 3. DANH SÁCH TEST CASE FAIL
TC16-01 (P1) - BUG-001: Thiếu cột "Interviewer"; cột "Result" không tách riêng mà gộp vào Status.
TC16-04 (P1) - BUG-002: Message thực tế "No Interview Schedule has been found" không khớp ME008.
TC16-16 (P1) - BUG-005: Không role nào có icon View ở cột Action.
TC16-17 (P2) - BUG-001: Không có cột "Result" độc lập, chỉ thấy "Passed"/"Failed" gộp trong Status, không có N/A.
TC16-19 (P1) - BUG-006: Format ngày hiện "DD-MM-YYYY" (gạch ngang) thay vì "DD/MM/YYYY" (gạch chéo) theo spec.
TC16-22 (P2) - BUG-001: Không có cột "Result" để kiểm tra giá trị khi Status = Cancelled.

## 4. TEST CASE BỊ BLOCK
TC16-05 (P1) - Chặn bởi BUG-003: không có combo-box "Interviewer" trong Search section.
TC16-06 (P2) - Chặn bởi BUG-003: không có cột/combo Interviewer.
TC16-07 (P1) - Chặn bởi BUG-004: thiếu status "New" trong dropdown (3 status còn lại: Invited/Interviewed/Cancelled vẫn lọc đúng).
TC16-09 (P1) - Không có quyền/công cụ truy cập API để test.
TC16-18 (P1) - Không có quyền chỉnh dữ liệu test (cần tạo Result bất thường để kiểm tra).
TC16-20 (P1) - Không có quyền tạo dữ liệu test sai định dạng.
TC16-21 (P1) - Chặn bởi BUG-003: Search + Status kết hợp hoạt động đúng, nhưng không test được đủ 3 filter do thiếu Interviewer.

## 5. NHẬN XÉT (AI viết)
**1. Nhận xét tổng quan về chất lượng**
Phần lớn các chức năng cơ bản của UC16 đã được kiểm thử trên cả bốn vai trò người dùng. Tuy nhiên, vẫn còn một số lượng đáng kể các trường hợp kiểm thử bị block do các lỗi chưa được khắc phục hoặc do giới hạn quyền truy cập/công cụ của tester. Một số lỗi liên quan đến giao diện và logic hiển thị dữ liệu (ví dụ: thiếu cột, format ngày, thông điệp không khớp đặc tả) vẫn còn tồn đọng, ảnh hưởng đến khả năng xác nhận tính đúng đắn của hệ thống theo yêu cầu nghiệp vụ. Ngoài ra, một số trường hợp kiểm thử đang tạm dừng do chờ xác nhận phạm vi nghiệp vụ từ phía BA.

**2. Điểm đang lo ngại nhất**
Điểm đáng lo ngại nhất là các lỗi Major hiện tại đang chặn phần lớn các trường hợp kiểm thử liên quan đến các filter quan trọng (Search, Interviewer, Status) và trường dữ liệu Result độc lập. Điều này khiến việc xác nhận tính đầy đủ và chính xác của chức năng View Interview Schedule chưa thể thực hiện một cách toàn diện. Bên cạnh đó, việc thiếu quyền truy cập API và không thể tạo dữ liệu test bất thường cũng hạn chế khả năng kiểm thử các tình huống biên và các trường hợp ngoại lệ.

**3. Rủi ro nếu tiếp tục phát hành**
Nếu tiếp tục phát hành trong tình trạng hiện tại, có nguy cơ cao hệ thống sẽ không đáp ứng đầy đủ các yêu cầu nghiệp vụ liên quan đến việc lọc, tìm kiếm và hiển thị thông tin lịch phỏng vấn. Một số chức năng quan trọng có thể hoạt động không đúng hoặc thiếu sót, dẫn đến trải nghiệm người dùng không nhất quán và tiềm ẩn lỗi nghiệp vụ. Ngoài ra, việc chưa kiểm thử được các trường hợp đặc biệt và các vai trò người dùng cụ thể có thể gây ra rủi ro về bảo mật và phân quyền.
**ĐỀ XUẤT - cho quyết định cuối cùng của người phụ trách.**

## 6. ĐÁNH GIÁ TIÊU CHÍ THOÁT

| Tiêu chí | Mục tiêu | Thực tế | Đạt/Không |
|---|---|---|---|
| Bug Critical đang mở | 0 | 0 | Đạt |
| Bug Major đang mở | ≤ 2 (đề xuất) | 4 | Không đạt |
| Pass rate (trên TC đã thực thi) | ≥ 80% (đề xuất) | 35% (7/20) | Không đạt |
| Execution rate (trên tổng TC thiết kế) | ≥ 95% (đề xuất) | 90.9% (20/22) | Không đạt |
| TC P0 | 100% Pass | 2/2 Pass (TC16-10, TC16-11) | Đạt |
| TC P1 | 100% Pass / không Blocked | 8 Blocked/Fail trong 14 TC P1 | Không đạt |
| TC đang PENDING chờ BA | 0 (phải resolve trước release) | 2 (TC16-14, TC16-15) | Không đạt |

> NGƯỜI REVIEW cần: (1) thay bảng "Mục tiêu" bằng số thật do BA/PM cung cấp, (2) tự đối chiếu lại cột Đạt/Không sau khi có số thật.

## 7. QUYẾT ĐỊNH

**Hỗ trợ ra quyết định — trả lời 6 câu hỏi:**

1. **Tất cả tiêu chí thoát đã đạt chưa?** → Chưa. 4/7 tiêu chí (theo ngưỡng đề xuất) chưa đạt.
2. **Còn bug Critical/Blocker nào đang mở không?** → Không có Critical, nhưng có 4 bug Major đang chặn nhiều TC quan trọng (BUG-001, BUG-003, BUG-004, BUG-005).
3. **Test case chưa chạy có thuộc chức năng quan trọng không?** → Có thể có — TC16-14, TC16-15 (P1) đang PENDING, chưa rõ mức độ quan trọng vì còn chờ BA xác nhận phạm vi role "HR".
4. **Rủi ro nếu phát hành:** → 3 filter chính (Search/Interviewer/Status) không dùng kết hợp được đầy đủ; trường Result không tách riêng khỏi Status; action View thiếu ở Role C/D; format ngày sai spec. Ảnh hưởng trực tiếp đến trải nghiệm và tính đúng đắn nghiệp vụ.
5. **Có workaround cho các lỗi còn tồn không?** → Chưa có thông tin — cần hỏi dev/PM trước khi quyết định.
6. **Nếu chọn GO with conditions — điều kiện cụ thể là gì?** → Đề xuất: fix xong 4 bug Major (BUG-001, BUG-003, BUG-004, BUG-005) trước khi release; 2 TC pending (TC16-14, TC16-15) phải có xác nhận từ BA.

**Đề xuất của AI dựa trên số liệu (KHÔNG phải quyết định chính thức):**
Nghiêng về **NO-GO**, hoặc **GO with conditions** nếu team cam kết fix 4 bug Major trước ngày release. Lý do: Pass rate 35% và 4 bug Major đang chặn các chức năng lọc/filter cốt lõi của UC16 — mức rủi ro cao nếu phát hành ngay trong tình trạng hiện tại.

---

- [ ] GO          - đồng ý phát hành
- [ ] NO-GO       - chưa phát hành
- [ ] GO with conditions - phát hành kèm điều kiện

Lý do quyết định: __________

Điều kiện kèm theo (nếu chọn GO with conditions): __________

Người quyết định: __________ Ngày: __________

> ⚠️ Mục 7 (tick chọn ô, lý do, người ký) BẮT BUỘC do người có thẩm quyền quyết định. AI chỉ đưa ra đề xuất dựa trên số liệu, không có thẩm quyền tự quyết GO/NO-GO.
