# 14. GO / NO-GO DECISION — UC16: View Interview Schedule
**Giai đoạn 4 — Quyết định release**

## 1. Căn cứ ra quyết định
Dựa trên `12_test_report.md` (Test Summary Report) và đối chiếu số liệu tại `13_number_check.md`:

| Tiêu chí | Mục tiêu | Thực tế | Đạt/Không |
|---|---|---|---|
| Bug Critical đang mở | 0 | 0 | Đạt |
| Bug Major đang mở | ≤ 2 | 4 | Không đạt |
| Pass rate (trên TC đã thực thi) | ≥ 80% | 35% (7/20) | Không đạt |
| Execution rate (trên tổng TC thiết kế) | ≥ 95% | 90.9% (20/22) | Không đạt |
| TC P0 | 100% Pass | 2/2 Pass | Đạt |
| TC P1 | 100% Pass / không Blocked | 8 Blocked/Fail trong 14 TC P1 | Không đạt |
| TC đang PENDING chờ BA | 0 | 2 (TC16-14, TC16-15) | Không đạt |

**5/7 tiêu chí thoát không đạt.**

## 2. Bug Major đang mở (chặn quyết định)
| Bug ID | Mô tả | TC bị ảnh hưởng |
|---|---|---|
| BUG-001 | Thiếu cột Interviewer/Result, Result gộp vào Status | TC16-01, 17, 22 |
| BUG-003 | Thiếu combo-box filter Interviewer | TC16-05, 06, 21 |
| BUG-004 | Dropdown Status thiếu "New", có giá trị lạ "Open" | TC16-07 |
| BUG-005 | Icon "View" không hiển thị cho bất kỳ role nào | TC16-16 |

Các bug này chặn trực tiếp 3 filter cốt lõi (Search/Interviewer/Status) và chức năng xem chi tiết — đều là chức năng chính của UC16, không phải edge case.

## 3. QUYẾT ĐỊNH

- [x] **NO-GO** — chưa phát hành
- [ ] GO — đồng ý phát hành
- [ ] GO with conditions — phát hành kèm điều kiện

**Lý do quyết định:**
Pass rate chỉ đạt 35% (7/20 TC đã thực thi), thấp hơn nhiều so với ngưỡng đề xuất 80%. Có 4 bug Major đang mở, chặn cả 3 filter chính (Search, Interviewer, Status) và trường Result — đây là các chức năng cốt lõi của UC16, không phải lỗi phụ. Ngoài ra còn 2 test case P1 (TC16-14, TC16-15) đang ở trạng thái PENDING vì chưa được BA xác nhận phạm vi role "HR", nghĩa là vẫn còn rủi ro chưa xác định về phân quyền. Với mức độ lỗi và độ phủ kiểm thử hiện tại, phát hành lúc này có rủi ro cao ảnh hưởng trực tiếp đến trải nghiệm người dùng và tính đúng đắn nghiệp vụ.

**Điều kiện để chuyển sang GO (retest sau khi fix):**
1. Fix xong 4 bug Major: BUG-001, BUG-003, BUG-004, BUG-005.
2. BA xác nhận chính thức role "HR" (Recruiter hay role riêng) để mở khóa TC16-14, TC16-15.
3. Chạy lại toàn bộ regression cho các TC liên quan: TC16-01, 05, 06, 07, 16, 17, 21, 22.
4. Đạt lại Pass rate ≥ 80% trên các TC đã thực thi trước khi xét lại quyết định.

**Người quyết định:** Nguyễn Lê Ngọc Bảo
**Ngày:** 13/08/2026
