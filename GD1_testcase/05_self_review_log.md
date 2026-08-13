# 05. SELF-REVIEW LOG — So sánh AI tự tìm vs Human tìm thêm (UC16)

## 1. AI Self-Review tự tìm được BAO NHIÊU vấn đề thật?

AI Self-Review (node 7, chạy trên bộ 20 test case) báo cáo **5 nhóm vấn đề**:

| # | Nhóm vấn đề AI báo cáo | Kết quả kiểm tra lại (Human) |
|---|---|---|
| 1 | Traceability — Source ghi mơ hồ ("Step X"/"Cond TC16-XX") cho 9 TC | ✅ **ĐÚNG** — xác nhận thật, đã sửa ở file 04 |
| 2 | Traceability — "TC16-03, TC16-04 dùng BRL-16-03 nhưng BRL-16-03 không tồn tại" | ❌ **SAI — AI bịa/nói nhầm.** BRL-16-03 **có tồn tại** ("The search box allows partial match on all columns"), đúng như trong PHẦN A của 02_risk_analysis.md. AI tự mâu thuẫn với chính output business rule mà nó đã sinh ra trước đó. |
| 3 | Priority — đề nghị nâng P2→P1 cho 5 TC (06, 08, 16, 17, 19) | ⚠️ **ĐÚNG MỘT PHẦN** — Human chỉ đồng ý nâng **2/5** (TC16-16, TC16-19), giữ nguyên P2 cho TC16-06, 08, 17 vì rủi ro thực tế thấp hơn AI đánh giá (xem lý do BA ở file 04). |
| 4 | Quality — Expected Result thiếu tiêu chí đo lường, cho 8 TC | ✅ **ĐÚNG** — đã bổ sung tiêu chí cụ thể ở file 04. |
| 5 | Quality — TC16-20 tiêu chí pass/fail không rõ | ✅ **ĐÚNG** — đã bổ sung rõ điều kiện PASS/FAIL ở file 04. |

**→ AI tìm đúng thực chất: 4/5 nhóm (nhóm #2 là kết luận sai/bịa đặt).**
Ở bước ambiguity analysis trước đó (node 2), AI cũng tìm ra **11/11 vấn đề hợp lệ** (0 invalid), trong đó 9 valid rõ ràng, 2 cần bàn thêm nhưng không sai bản chất.

## 2. AI có BỎ SÓT vấn đề nào mà học viên thấy? (so sánh 2 con số)

| Phạm vi | Số vấn đề AI tự tìm (thật, đã trừ phần sai) | Số vấn đề học viên tìm thêm |
|---|---|---|
| Ambiguity analysis (node 2) | 11 | **2** (cột "Job" không có spec; xung đột role "HR" vs "Recruiter") |
| Self-review test case (node 7) | 4 | **3** (1 - phát hiện AI nói sai về BRL-16-03; 2 - gap coverage: thiếu test kết hợp filter dù AI kết luận "không gap"; 3 - Role A/B/C/D trong toàn bộ TC không có bảng mapping sang role thật) |
| **Tổng** | **15** | **5** |

**Ghi lại (so sánh quan trọng):**
- (a) Số lỗi AI TỰ tìm được: **15** (11 ở node 2 + 4 ở node 7)
- (b) Số lỗi học viên tìm thêm: **5**
- Ngoài ra: AI có **1 kết luận sai/bịa** (BRL-16-03) — nếu không có Human Review, đội test sẽ tốn công "sửa" một Source vốn dĩ đã đúng.

## 3. AI có bịa vấn đề giả tạo hoặc sửa "review sai" không?

**Có, 1 trường hợp:** AI Self-Review khẳng định BRL-16-03 "không tồn tại" trong khi rule này đã được chính AI sinh ra trước đó (ở bước phân tích business rules). Đây là lỗi tự mâu thuẫn giữa các bước của AI (không kiểm tra chéo output cũ), không phải hallucination hoàn toàn từ hư không — nhưng hậu quả thực tế giống nhau: nếu tin theo, người review sẽ sửa nhầm một Source đang đúng.

## 4. Kiểm tra bảng Traceability — có GAP nào không?

AI tự đánh giá "Không phát hiện GAP về coverage". **Human Review không đồng ý hoàn toàn:**
- **GAP thật sự bị bỏ sót:** không có test case nào kiểm tra việc **kết hợp đồng thời** Search box + Interviewer + Status (chỉ test riêng lẻ từng filter) → đã bổ sung TC16-21.
- **GAP thật sự bị bỏ sót:** cột **"Job"** xuất hiện trên mock-up nhưng không có trong bảng field spec lẫn bảng traceability của AI → 0% coverage, và AI không hề nhắc tới trong toàn bộ báo cáo self-review.
- Bảng Traceability của AI liệt kê 10 hạng mục "COVERED" — con số này **đúng nhưng không đầy đủ**, vì thiếu hẳn 2 hạng mục (combined filter, cột Job) mà AI chưa từng đưa vào danh sách để đánh giá.

## 5. Test case nào phải sửa? Sửa gì?

| TC ID | Sửa gì |
|---|---|
| TC16-01, 02, 05, 06, 08, 09, 16, 19, 20 | Source cụ thể hóa (trỏ đúng REF/BRL/UC step) |
| TC16-01, 05, 06, 07, 08, 16, 17, 20 | Expected Result bổ sung tiêu chí đo lường cụ thể |
| TC16-16, TC16-19 | Priority: P2 → P1 |
| TC16-14, TC16-15 | Gắn trạng thái PENDING, chờ BA xác nhận role "HR" |
| — (mới) TC16-21, TC16-22 | Bổ sung mới, bù gap coverage |
| — (mới) TC16-Job | Không viết được, đánh dấu BLOCKED chờ BA bổ sung field spec |

## 6. Ghi lại: số TC AI sinh / số TC phải sửa

- **Số TC AI sinh ban đầu:** 20
- **Số TC phải sửa sau Human Review:** 13/20 (65%)
- **Số TC bổ sung mới sau Human Review:** 2 (TC16-21, TC16-22)
- **Số TC bị block, chưa viết được:** 1 (cột Job)
- **Số TC ở trạng thái chờ xác nhận (PENDING):** 2 (TC16-14, TC16-15)

## 7. Chốt bản test case cuối cùng
→ Xem `04_test_cases_final.md` (22 test case: 20 gốc đã hiệu chỉnh + 2 mới), với ghi chú PENDING/BLOCKED rõ ràng cho các mục còn chờ BA.

---

## KẾT LUẬN: "AI Self-Review có thay được Human Review không?"

**Không.** Bằng chứng cụ thể từ UC16:

1. **Tỷ lệ phát hiện:** AI tự tìm được 15 vấn đề hợp lệ, nhưng vẫn bỏ sót **5 vấn đề** mà Human tìm thêm — tỷ lệ bỏ sót ~25% so với tổng số vấn đề thật (20 vấn đề hợp lệ được xác nhận).
2. **False positive:** AI đưa ra **1 kết luận sai** (BRL-16-03) mà nếu không kiểm tra chéo, đội test sẽ sửa nhầm chỗ đúng.
3. **Vấn đề nghiêm trọng nhất bị bỏ sót lại chính là vấn đề rủi ro cao nhất:** cột "Job" — field hiển thị thật trên UI, không có spec, không có test case, không được AI nhắc đến ở bất kỳ bước nào (ambiguity analysis, business rules, hay self-review). Đây là loại lỗi mà chỉ có người đối chiếu trực tiếp mock-up với bảng field spec mới phát hiện được.
4. **Xung đột thuật ngữ (HR vs Recruiter)** ảnh hưởng trực tiếp đến toàn bộ ma trận phân quyền test (Role A/B/C/D) nhưng AI không phát hiện vì nó đòi hỏi đối chiếu chéo giữa nhiều phần tài liệu (Actor list, Screen Description, Business Rule) — việc mà AI xử lý tuần tự từng phần dễ bỏ sót.

→ AI Self-Review có giá trị thật (tiết kiệm thời gian rà soát các lỗi "bề mặt": source thiếu, expected result mơ hồ), nhưng **không thay thế được** vai trò Human Review trong việc: (a) phát hiện gap dữ liệu/field hoàn toàn chưa được đặc tả, (b) phát hiện xung đột thuật ngữ xuyên suốt tài liệu, và (c) kiểm tra chéo để bắt lỗi tự mâu thuẫn của chính AI.