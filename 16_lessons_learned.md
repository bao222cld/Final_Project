# 16. LESSONS LEARNED — UC16: View Interview Schedule
**Tổng kết bài học sau toàn bộ workflow AI-for-Tester (GĐ0 → GĐ4)**

---

## 1. Về việc dùng AI trong quy trình test

**Điều AI làm tốt:**
- Sinh nhanh số lượng lớn artifact có cấu trúc: 11 điểm mơ hồ ở bước ambiguity analysis, 20 test case ban đầu, 34 giả thuyết root cause cho 5 bug — tất cả đúng format, đủ cột yêu cầu, không cần con người soạn từ đầu.
- Đưa ra nhiều giả thuyết root cause song song thay vì chốt ngay 1 hướng — giúp tránh thiên kiến "nghĩ tới đâu tin tới đó" thường gặp khi người mới làm QA tự điều tra bug.
- Áp dụng ràng buộc chặt khi được cấu hình đúng (ví dụ Agent GD3 luôn phải điền cột "Evidence loại trừ", luôn ghi rõ "THIẾU EVIDENCE" thay vì bịa) — cho thấy AI tuân thủ tốt instruction khi instruction đủ cụ thể và có ví dụ rõ ràng.

**Điều AI làm chưa tốt:**
- AI xử lý tài liệu tuần tự theo từng bước, không tự đối chiếu chéo hiệu quả giữa các phần tài liệu khác nhau (Actor list vs Screen Description vs Business Rule) → bỏ sót xung đột thuật ngữ "HR" vs "Recruiter".
- AI không tự "nhớ" chính xác nội dung nó đã sinh ra ở bước trước, dẫn đến 1 lần tự mâu thuẫn (khẳng định sai BRL-16-03 không tồn tại, dù chính AI đã sinh ra rule này).
- AI không phát hiện được field hoàn toàn chưa có đặc tả (cột "Job" xuất hiện trên mock-up nhưng không có trong bảng field spec) — loại lỗi này đòi hỏi đối chiếu trực tiếp hình ảnh mock-up với bảng đặc tả, một việc AI không chủ động làm nếu không được yêu cầu rõ.

---

## 2. Về vai trò của Human Review

- Human Review không thể bị thay bằng AI Self-Review, kể cả khi AI Self-Review được thiết kế bài bản: tỷ lệ AI bỏ sót vẫn ở mức ~25% (5/20 vấn đề hợp lệ) trong dự án này.
- Giá trị lớn nhất của Human Review không nằm ở việc bắt lỗi bề mặt (source thiếu, format sai) — AI làm việc này khá tốt — mà nằm ở:
  1. Phát hiện gap dữ liệu hoàn toàn chưa được đặc tả (field "Job").
  2. Phát hiện xung đột thuật ngữ xuyên suốt nhiều phần tài liệu (HR vs Recruiter).
  3. Kiểm tra chéo để bắt lỗi tự mâu thuẫn của chính AI (BRL-16-03).
- Việc yêu cầu AI luôn đánh dấu rõ trạng thái "PENDING" khi có điểm chưa xác nhận (ví dụ TC16-14, TC16-15 chờ BA xác nhận role "HR") giúp Human Review không bị cuốn theo giả định sai của AI — đây là một pattern nên áp dụng lại cho các dự án sau.

---

## 3. Về đo lường hiệu quả (Effective ROI / Hallucination Rate)

- ROI đo được ở dự án này là ~43%, không cao vượt trội nhưng dương và ổn định qua các bước — cho thấy AI tiết kiệm thời gian thật ngay cả khi phải cộng thêm thời gian human review nghiêm túc, không phải chỉ tiết kiệm "trên giấy".
- Hallucination Rate thấp (~0.9%, 1/109 item) không có nghĩa là AI "an toàn" tuyệt đối — vì hallucination duy nhất xảy ra lại ở đúng bước Self-Review, nơi lẽ ra AI phải đáng tin cậy nhất (đang tự kiểm tra công việc của chính nó). Điều này cho thấy tần suất thấp không đồng nghĩa rủi ro thấp; cần nhìn vào **vị trí** hallucination xảy ra, không chỉ con số %.
- Bài học đo lường: nên ghi số liệu (AI time, human review time, item phải sửa) **ngay trong lúc làm**, không ước lượng lại sau khi xong — vì ước lượng ngược dễ bị thiên kiến (nhớ nhầm thời gian, đánh giá thấp công sức review thực tế đã bỏ ra).

---

## 4. Về thiết kế workflow (Hybrid AI + Human)

- Việc chia workflow thành nhiều "cổng chặn" Human Review giữa các giai đoạn (sau ambiguity analysis, sau khi sinh test case, sau khi có giả thuyết root cause, trước khi ra bug report) là yếu tố quan trọng nhất giúp hallucination không lan sang giai đoạn sau — không có lỗi nào trong dự án này lọt tới tận Test Report hay Go/No-Go decision mà chưa qua ít nhất một vòng review.
- Việc tách rõ vai trò AI ("chỉ đưa giả thuyết, không kết luận root cause khi chưa verify", "chỉ sinh nội dung Jira ticket, không tự tạo ticket thật") giúp giới hạn đúng phạm vi trách nhiệm của AI, tránh trường hợp AI tự quyết định thay con người ở những việc có ảnh hưởng thật (ví dụ ghi thẳng vào hệ thống Jira, hay tự chọn GO/NO-GO).

---

## 5. Đề xuất cho dự án tiếp theo

1. Thêm bước bắt buộc "trích dẫn nguyên văn" mỗi khi AI tham chiếu lại một artifact nó đã tự sinh trước đó, để giảm rủi ro tự mâu thuẫn (loại hallucination "trace sai nguồn").
2. Bổ sung một bước riêng "đối chiếu mock-up với field spec" bằng checklist tường minh, thay vì kỳ vọng AI tự phát hiện field thiếu spec.
3. Ghi số liệu đo lường (AI time/human review/item sửa) theo thời gian thực trong từng bước, tránh ước lượng hồi cứu.
4. Giữ nguyên nguyên tắc: AI chỉ đề xuất, con người luôn là người ký quyết định cuối cùng (đã áp dụng đúng ở bước Go/No-Go decision).
