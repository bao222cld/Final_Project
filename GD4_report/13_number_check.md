# 13_NUMBER_CHECK — Đối chiếu số liệu Report vs Data gốc
**UC16 — View Interview Schedule**

> Mục đích: đối chiếu TỪNG con số trong Test Summary Report với Execution Log gốc, để đảm bảo AI không bịa/lệch số liệu. Đây là bước bắt buộc trong HR5 trước khi review nhận xét và ra quyết định.

## 1. Đối chiếu số liệu thực thi (Mục 1 của Report)

| Chỉ số | Report ghi | Đếm lại từ Execution Log | Khớp? |
|---|---|---|---|
| Tổng test case | 22 | 22 (không tính TC16-Job) | Khớp |
| Đã thực thi | 20 | 20 (7 Pass + 6 Fail + 7 Blocked) | Khớp |
| Pass | 7 | TC16-10, 11, 02, 03, 12, 13, 08 = 7 | Khớp |
| Fail | 6 | TC16-01, 04, 16, 17, 19, 22 = 6 | Khớp |
| Blocked | 7 | TC16-05, 06, 07, 09, 18, 20, 21 = 7 | Khớp |
| Chưa chạy (không nằm trong Report nhưng cần lưu ý) | — | TC16-14, TC16-15 = 2 (PENDING) | Ghi chú riêng |

**Kiểm tra tổng:** 7 + 6 + 7 = 20 (Đã thực thi) | 20 + 2 (chưa chạy) = 22 (Tổng thiết kế) 

## 2. Đối chiếu tình trạng bug (Mục 2 của Report)

| Mức độ | Report ghi | Đếm lại từ Execution Log | Khớp? |
|---|---|---|---|
| Critical | 0 | Không có bug nào gắn mức Critical | Khớp |
| Major | 4 | BUG-001, BUG-003, BUG-004, BUG-005 | Khớp |
| Minor | 2 | BUG-002, BUG-006 | Khớp |
| **Tổng bug** | 6 | 6 | Khớp |

## 3. Đối chiếu danh sách TC Fail (Mục 3 của Report)

| TC ID | Report có liệt kê? | Có đúng trong Execution Log không? | Bug ID khớp? |
|---|---|---|---|
| TC16-01 | ✅ | ✅ Fail, BUG-001 | ✅ |
| TC16-04 | ✅ | ✅ Fail, BUG-002 | ✅ |
| TC16-16 | ✅ | ✅ Fail, BUG-005 | ✅ |
| TC16-17 | ✅ | ✅ Fail, BUG-001 | ✅ |
| TC16-19 | ✅ | ✅ Fail, BUG-006 | ✅ |
| TC16-22 | ✅ | ✅ Fail, BUG-001 | ✅ |

Số lượng: Report liệt kê 6 TC Fail — khớp đúng với số 6 ở Mục 1. 

## 4. Đối chiếu danh sách TC Blocked (Mục 4 của Report)

| TC ID | Report có liệt kê? | Có đúng trong Execution Log không? | Bug ID khớp? |
|---|---|---|---|
| TC16-05 | ✅ | ✅ Blocked, BUG-003 | ✅ |
| TC16-06 | ✅ | ✅ Blocked, BUG-003 | ✅ |
| TC16-07 | ✅ | ✅ Blocked, BUG-004 | ✅ |
| TC16-09 | ✅ | ✅ Blocked, không có Bug ID | ✅ |
| TC16-18 | ✅ | ✅ Blocked, không có Bug ID | ✅ |
| TC16-20 | ✅ | ✅ Blocked, không có Bug ID | ✅ |
| TC16-21 | ✅ | ✅ Blocked, BUG-003 | ✅ |

Số lượng: Report liệt kê 7 TC Blocked — khớp đúng với số 7 ở Mục 1. 

## 5. Kiểm tra phần Nhận xét (Mục 5 của Report — do AI viết)

| Yêu cầu ràng buộc | Kết quả kiểm tra |
|---|---|
| Không chứa con số nào | Đạt — chỉ dùng định tính ("phần lớn", "một số lượng đáng kể") |
| Không tự quyết GO/NO-GO | Đạt — có ghi rõ "ĐỀ XUẤT - cho quyết định cuối cùng của người phụ trách" |
| Đúng 3 phần yêu cầu | Đạt — Tổng quan chất lượng / Điểm lo ngại / Rủi ro nếu phát hành |

## 6. Tính toán bổ sung (phục vụ bảng exit criteria ở Report Mục 6)

| Chỉ số tính toán | Công thức | Kết quả |
|---|---|---|
| Pass rate (trên TC đã thực thi) | 7 / 20 | 35% |
| Execution rate (trên tổng TC thiết kế) | 20 / 22 | 90.9% |
| Tỷ lệ Fail (trên TC đã thực thi) | 6 / 20 | 30% |
| Tỷ lệ Blocked (trên TC đã thực thi) | 7 / 20 | 35% |
| Bug Major / tổng bug | 4 / 6 | 66.7% |

## 7. Kết luận đối chiếu

- [x] Toàn bộ số liệu trong Report (Mục 1, 2, 3, 4) đã được đối chiếu và **khớp 100%** với Execution Log gốc.
- [x] Phần Nhận xét (Mục 5) tuân thủ đúng ràng buộc, không có số liệu bịa/tự quyết định.
- [ ] **Người review cần tự xác nhận lại bằng mắt** — bảng trên do AI hỗ trợ đối chiếu, không thay thế được việc người review tự kiểm tra trực tiếp Execution Log gốc.

**Người đối chiếu:** __________
**Ngày đối chiếu:** __________
**Kết luận:** [ ] Số liệu ĐÚNG, có thể dùng để ra quyết định &nbsp;&nbsp; [ ] Có sai lệch, cần sửa lại report trước khi ra quyết định
