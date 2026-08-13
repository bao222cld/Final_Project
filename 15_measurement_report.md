# 15. MEASUREMENT REPORT — 3 Chỉ Số Hiệu Quả AI
**UC16: View Interview Schedule**

> Số liệu trong file này là **ước lượng giả định** (học viên tự ghi nhận trong lúc làm, không phải ước lượng ngược lại sau khi xong), dùng để minh hoạ cách tính 3 chỉ số bắt buộc.

---

## 16. Ba chỉ số bắt buộc

| # | Chỉ số | Công thức | Ý nghĩa |
|---|---|---|---|
| 1 | Effective Time Saved | Manual est. – (AI time + Human review) | Thời gian tiết kiệm THỰC (số phút) |
| 2 | Effective ROI | Effective Time Saved / Manual est. × 100% | Tỷ lệ tiết kiệm thực (%) |
| 3 | Hallucination Rate | Số lần AI bịa / Tổng item AI sinh × 100% | Tỷ lệ AI bịa thông tin sai |

---

## 17. Bảng thu thập số liệu

**Giai đoạn 2 (thực thi thủ công) KHÔNG đưa vào bảng này** — đây là công việc tay, không có AI tham gia, nếu tính vào sẽ làm lệch ROI của AI.

| Bước | AI time (phút) | Human review (phút) | Manual est. (phút) | Item AI sinh | Item phải sửa | Số lần AI bịa |
|---|---:|---:|---:|---:|---:|---:|
| GD1 - Tìm điểm mơ hồ | 5 | 15 | 30 | 11 | 2 | 0 |
| GD1 - Risk analysis | 4 | 10 | 20 | 8 | 1 | 0 |
| GD1 - Test conditions | 5 | 12 | 25 | 20 | 5 | 0 |
| GD1 - Test cases (kèm self-review) | 8 | 25 | 60 | 20 | 13 | 1 |
| GD2 - Phân tích bug | 6 | 15 | 40 | 5 | 0 | 0 |
| GD2 - Hypotheses | 10 | 20 | 60 | 34 | 0 | 0 |
| GD2 - Bug report + Jira | 8 | 15 | 45 | 10 | 2 | 0 |
| GD3 - Test report | 4 | 10 | 20 | 1 | 0 | 0 |
| **TỔNG** | **50** | **122** | **300** | **109** | **23** | **1** |

**Ghi chú nguồn số liệu:**
- Item AI sinh/phải sửa của "Test conditions" và "Test cases" lấy theo `04_test_cases_final.md` (22 TC, 13/20 TC gốc phải sửa) và `05_self_review_log.md`.
- 1 lần AI bịa duy nhất: AI Self-Review khẳng định sai rằng `BRL-16-03` "không tồn tại" trong khi chính AI đã sinh ra rule này trước đó (xem `05_self_review_log.md`, mục 3).
- "Hypotheses" tính là 34 item (5 bug × ~6-7 giả thuyết/bug theo `08_hypotheses.md`).

---

## 18. Cách đếm Hallucination

| Loại | Có tính là hallucination? | Ví dụ trong dự án |
|---|---|---|
| Bịa requirement/rule | **CÓ** | Sinh test case dựa vào chức năng không có trong UC |
| Bịa dòng log | **CÓ** | Trích dẫn log không tồn trong evidence gốc |
| Bịa số liệu | **CÓ** | Report ghi pass rate 95% trong khi dữ liệu thật là 70% |
| Trace sai nguồn | **CÓ** | AI Self-Review khẳng định `BRL-16-03` không tồn tại — đúng loại này, đã tính vào bảng trên |
| Diễn đạt chưa hay | KHÔNG | Tính vào "item phải sửa" |
| Thiếu chi tiết | KHÔNG | Tính vào "item phải sửa" |
| Format sai | KHÔNG | Tính vào "item phải sửa" |

> Phân biệt rõ: "item phải sửa" bao gồm CẢ hallucination lẫn lỗi nhẹ. Còn "hallucination" chỉ tính riêng. Vì vậy "item phải sửa" luôn ≥ số hallucination. Trong bảng ở mục 17, số hallucination (1) đã nằm trong số item phải sửa (13) của bước "GD1 - Test cases".

---

## 19. Phân tích rủi ro của Hallucination

| Hallucination phát hiện được | Ở bước nào | Nếu KHÔNG phát hiện thì sao? | Mức độ rủi ro |
|---|---|---|---|
| AI khẳng định sai `BRL-16-03` "không tồn tại" (Self-Review, node 7) | GD1 - Test cases (self-review) | Học viên sẽ tưởng Source của TC16-03/04 bị sai và **sửa nhầm một chỗ vốn đã đúng** — tốn công vô ích, có thể làm mất traceability thật của rule này | TRUNG BÌNH |

**Thang đánh giá rủi ro:**

| Mức | Tiêu chí |
|---|---|
| CAO | Lọt ra production, ảnh hưởng khách hàng |
| TRUNG BÌNH | Gây lãng phí công sức, phải làm lại |
| THẤP | Dễ phát hiện, sửa nhanh |

Trường hợp `BRL-16-03` được xếp **TRUNG BÌNH**: không lọt ra production (vì đã bắt được ở bước Human Review nội bộ), nhưng nếu không có bước đối chiếu chéo với chính output trước đó của AI (02_risk_analysis.md), học viên đã tốn công sửa nhầm.

---

## 20. Câu hỏi phân tích bắt buộc

**1. Giai đoạn nào AI hiệu quả NHẤT? Vì sao?**
"GD2 - Hypotheses" hiệu quả nhất: AI sinh 34 giả thuyết root cause đầy đủ 6 cột (Rank/Hypothesis/Xác suất/Evidence ủng hộ/Evidence loại trừ/Cách verify) trong 10 phút AI time, chỉ cần 20 phút human review mà không có item nào phải sửa hay bịa. Việc sinh nhiều giả thuyết có cấu trúc rõ ràng là việc AI làm nhanh và ít sai hơn con người vì nó không bị "đóng khung" vào 1-2 giả thuyết quen thuộc như người mới làm QA hay mắc phải.

**2. Giai đoạn nào AI hiệu quả KÉM NHẤT? Vì sao?**
"GD1 - Test cases" kém hiệu quả nhất: 13/20 item phải sửa (65%) và là nơi duy nhất xảy ra hallucination. Nguyên nhân: viết test case đòi hỏi AI phải tự suy luận Expected Result cụ thể, đo lường được (không chỉ mô tả chung chung), và đối chiếu ngược lại với chính các business rule nó đã sinh trước đó ở bước risk analysis — đây là bước dễ xảy ra tự mâu thuẫn nội bộ (self-consistency) nhất trong toàn bộ workflow.

**3. Khâu nào tốn Human review nhiều nhất? Có cách nào giảm không?**
"GD1 - Test cases" tốn nhiều nhất (25 phút). Có thể giảm bằng cách: (a) yêu cầu AI tự đối chiếu chéo với output các bước trước (risk analysis, ambiguity analysis) trước khi đưa ra bản test case, thay vì để người review tự phát hiện mâu thuẫn; (b) tách nhỏ review thành 2 vòng — review nhanh format/traceability trước, review nội dung logic sau — để không phải đọc lại toàn bộ 20 TC cùng lúc mỗi khi có thay đổi.

**4. AI bịa nhiều nhất ở đâu? Vì sao?**
Chỉ có 1 lần bịa duy nhất, ở bước Self-Review test case (trace `BRL-16-03`). Nguyên nhân: AI xử lý các bước tuần tự (sequential), không thực sự "nhớ" và đối chiếu lại chính xác nội dung nó đã sinh ra ở bước trước — dẫn đến tự mâu thuẫn giữa hai lần output khác nhau của cùng một agent.

**5. So sánh: số lỗi AI TỰ tìm ra vs số lỗi Human phải sửa/bổ sung? Điều đó nói lên điều gì về khả năng AI Self-Review?**
Theo `05_self_review_log.md`: AI tự tìm đúng được 15 vấn đề hợp lệ (11 ở ambiguity analysis + 4 ở self-review), trong khi Human tìm thêm 5 vấn đề AI bỏ sót (gap coverage khi kết hợp filter, field "Job" hoàn toàn chưa có spec, xung đột role "HR"/"Recruiter", v.v.), cộng thêm phát hiện 1 lần AI tự mâu thuẫn. Tỷ lệ bỏ sót ~25% (5/20 vấn đề hợp lệ thật). Điều này cho thấy AI Self-Review có giá trị thật (bắt được lỗi bề mặt: source thiếu, expected result mơ hồ) nhưng **không thể thay thế Human Review** ở việc đối chiếu chéo nhiều nguồn tài liệu cùng lúc và phát hiện gap dữ liệu hoàn toàn chưa được đặc tả — đây là loại lỗi đòi hỏi tư duy tổng hợp mà AI xử lý tuần tự dễ bỏ sót.

**6. Nếu Effective ROI thấp hoặc âm — nguyên nhân do đâu?**
Không áp dụng trong dự án này: Effective ROI tính được là **42.7%** (dương), tức AI vẫn tiết kiệm được ~43% thời gian so với làm thủ công hoàn toàn, dù đã cộng cả thời gian human review. Nếu giả sử ROI âm thì nguyên nhân thường gặp sẽ là: Manual est. bị ước lượng thấp hơn thực tế (nên khi cộng AI time + review time vượt quá), hoặc human review phải sửa quá nhiều khiến thời gian review gần bằng/hơn cả làm tay từ đầu — trường hợp này đúng nhất ở bước "GD1 - Test cases" nơi tỷ lệ phải sửa cao (65%).

**7. Với dữ liệu bạn có, bạn sẽ đề xuất gì cho AI khâu nào cần AI khác/review kỹ hơn? Vì sao?**
Đề xuất giữ nguyên cách dùng AI cho các bước sinh giả thuyết (Hypotheses) và bug report/Jira — đây là nơi AI hiệu quả cao, ít sai. Riêng bước "Test cases" và "Self-Review", nên bổ sung một bước bắt buộc: mỗi khi AI tự nhận xét/trace về một business rule hay artifact đã sinh trước đó, phải yêu cầu AI trích dẫn lại nguyên văn artifact đó trong cùng lượt trả lời (thay vì chỉ nói "tồn tại" hay "không tồn tại") — cách này biến việc kiểm tra chéo thành việc dễ đối chiếu bằng mắt, giảm rủi ro loại hallucination "trace sai nguồn" như đã xảy ra với `BRL-16-03`.

---

## Kết quả tính toán

| Chỉ số | Công thức | Kết quả |
|---|---|---|
| **Effective Time Saved** | 300 – (50 + 122) | **128 phút** |
| **Effective ROI** | 128 / 300 × 100% | **42.7%** |
| **Hallucination Rate** | 1 / 109 × 100% | **0.9%** |

**Nhận xét:** ROI dương ở mức trung bình-khá (~43%), hallucination rate rất thấp (dưới 1%, chỉ 1/109 item). Điều này phù hợp với đặc thù dự án: workflow có nhiều bước Human Review chặn giữa các giai đoạn, nên hallucination bị bắt sớm trước khi lan sang bước sau — đúng như mục đích thiết kế workflow hybrid (AI + Human review) của dự án.
