# Tổng hợp Bug Report – UC16 Interview Schedule

> Tổng hợp các Bug Report đã verify từ BUG-INTERVIEW-001 đến BUG-INTERVIEW-005.
> Root Cause chỉ được ghi là đã xác nhận khi người dùng đã cung cấp kết quả verify tương ứng.

---

# BUG 1 – Thiếu cột Interviewer và Result

## 1. Thông tin định danh
- Bug ID: `BUG-INTERVIEW-001`
- Title: Thiếu cột Interviewer và Result trên màn hình Interview Schedule
- Report Date: 13/08/2026
- Status: Open

## 2. Phân loại
- Severity: **Major** (đề xuất)
- Priority: **P1** (đề xuất)
- Bug Type: UI / Functional
- Component: Interview Schedule / Interview Schedule List
- Affects Version: N/A - cần bổ sung

## 3. Môi trường
- Browser: Chrome
- Environment / Build / OS: N/A - cần bổ sung

## 4. Mô tả lỗi

### Steps to Reproduce
1. Đăng nhập vào hệ thống với tài khoản có quyền truy cập Interview Schedule.
2. Vào màn hình `Interview Schedule`.
3. Quan sát header và các cột của bảng danh sách.
4. Đối chiếu với UC16 / REF 10 và REF 11.

### Repro Rate
N/A - số liệu 19/20 là số liệu verify H1, không được coi là Repro Rate độc lập.

### Expected Result
Bảng phải có các cột theo specification, bao gồm `Interviewer` và `Result`.

TC16-01 yêu cầu đủ 8 cột: Title, Candidate Name, Interviewer, Schedule, Result, Status, Job, Action.

### Actual Result
- Cột `Interviewer` không được hiển thị.
- Cột `Result` không được hiển thị.

## 5. Evidence
- Log: Không có / không truy cập được.
- DB / State: N/A.
- Evidence bổ sung cần có: screenshot hiện trạng, build/version cụ thể và evidence sau fix.

## 6. Root Cause

**Đã VERIFIED.**

> FE thiếu cấu hình 2 cột `Interviewer` và `Result` trong component render bảng Interview Schedule; table column definition chưa được cập nhật theo UC16.

### Kết quả verify
| Hypothesis | Kết quả | Số liệu |
|---|---|---:|
| H1 | **Confirmed** | 19/20 |
| H2 | **Rejected** | 3/20 |
| H3 | **Rejected** | 1/20 |

## 7. Trace
- Primary Test Case: **TC16-01**
- Related Test Case: **TC16-17**
- Requirement: **UC16 / REF 10 / REF 11**

## 8. Suggested Fix
1. Bổ sung column `Interviewer`.
2. Bổ sung column `Result`.
3. Mapping Interviewer tới tên interviewer.
4. Mapping Result tới `Passed`, `Failed` hoặc `N/A`.
5. Regression: TC16-01, TC16-17, TC16-18.

---

# BUG 2 – Sai message khi Search không có kết quả

## 1. Thông tin định danh
- Bug ID: `BUG-INTERVIEW-002`
- Title: Search không có kết quả hiển thị sai message ME008
- Report Date: 13/08/2026
- Status: Open

## 2. Phân loại
- Severity: **Major** (đề xuất)
- Priority: **P1** (đề xuất)
- Bug Type: Functional / UI
- Component: Interview Schedule
- Affects Version: N/A - cần bổ sung

## 3. Môi trường
- Browser: Chrome
- Environment / Build / OS: N/A - cần bổ sung

## 4. Mô tả lỗi

### Steps to Reproduce
1. Đăng nhập vào hệ thống.
2. Truy cập `Interview Schedule`.
3. Nhập keyword không tồn tại vào Search box.
4. Click `Search`.
5. Quan sát message.

### Repro Rate
**20/20** theo kết quả verify H1.

### Expected Result
Hiển thị đúng ME008:

`No items match with your search data. Please try again`

và bảng kết quả trống.

### Actual Result
Hệ thống hiển thị:

`No Interview Schedule has been found`

thay vì message ME008.

## 5. Evidence
- Screenshot: cần bổ sung tên file cụ thể.
- Log: Không truy cập được.
- DB / State: N/A.
- Evidence bổ sung: source/component đang cung cấp message, message key và request/response nếu cần.

## 6. Root Cause

**Đã VERIFIED.**

> FE hard-code hoặc sử dụng sai message key cho trạng thái Search không có kết quả, khiến UI hiển thị `No Interview Schedule has been found` thay vì ME008 `No items match with your search data. Please try again`.

### Kết quả verify
| Hypothesis | Kết quả | Số liệu |
|---|---|---:|
| H1 | **Confirmed** | 20/20 |
| H2 | **Confirmed** | 17/20 |
| H5 | **Rejected** | 1/10 |

## 7. Trace
- Test Case: **TC16-04**
- Requirement: **UC16 REF 6 / ME008**

> Nếu các artifact khác nhau ghi `No item...` và `No items...`, BA/QA cần thống nhất message chuẩn. Report này sử dụng bản ME008 được cung cấp: `No items match with your search data. Please try again`.

## 8. Suggested Fix
1. Kiểm tra message key trong flow Search → no result.
2. Sửa mapping về đúng ME008.
3. Tránh hard-code message trực tiếp nếu hệ thống có centralized message/i18n configuration.
4. Regression TC16-04 và Search flows liên quan.

---

# BUG 3 – Thiếu filter Interviewer

## 1. Thông tin định danh
- Bug ID: `BUG-INTERVIEW-003`
- Title: Thiếu filter Interviewer trên màn hình Interview Schedule
- Report Date: 13/08/2026
- Status: Open

## 2. Phân loại
- Severity: **Major** (đề xuất)
- Priority: **P1** (đề xuất)
- Bug Type: Functional
- Component: Interview Schedule
- Affects Version: N/A - cần bổ sung

## 3. Môi trường
- Browser: Chrome
- Environment / Build / OS: N/A - cần bổ sung

## 4. Mô tả lỗi

### Steps to Reproduce
1. Đăng nhập với account admin/IT hoặc interviewer/IT.
2. Vào `Interview Schedule`.
3. Quan sát khu vực Search/Filter.
4. Kiểm tra các control filter.
5. Thử thực hiện filter theo Interviewer.

### Repro Rate
**30/30** theo kết quả verify H1.

### Expected Result
Search section phải có combo-box `Interviewer`.

Theo UC16 REF 4:
- Chọn interviewer → chỉ hiển thị các lịch có interviewer tương ứng.
- Để trống → không áp dụng filter Interviewer.

### Actual Result
Khu vực filter không có combo-box/field `Interviewer`.

## 5. Evidence
- Screenshot: `screenshot_05`, `screenshot_06`, `screenshot_21`
- Log: Không truy cập được.
- DB / State: Không áp dụng cho symptom UI.
- Evidence bổ sung: source/component FE của Search section; API contract nếu danh sách interviewer lấy từ BE.

## 6. Root Cause

**Đã VERIFIED.**

> FE chưa implement component/field `Interviewer` trong Search section của màn hình Interview Schedule.

### Kết quả verify
| Hypothesis | Kết quả | Số liệu |
|---|---|---:|
| H1 | **Confirmed** | 30/30 |
| H2 | **Rejected** | 2/20 |
| H5 | **Rejected** | 0/10 (ON) |

## 7. Trace
- Test Cases: **TC16-05, TC16-06, TC16-21**
- Requirement: **UC16 REF 4**

## 8. Suggested Fix
1. Implement Interviewer combo-box.
2. Populate danh sách interviewer phù hợp.
3. Khi Interviewer blank, không áp dụng điều kiện filter.
4. Tích hợp Interviewer với logic Search.
5. Regression TC16-05, TC16-06 và TC16-21.

---

# BUG 4 – Status “Open” thay vì “New”

## 1. Thông tin định danh
- Bug ID: `BUG-INTERVIEW-004`
- Title: Status dropdown hiển thị `Open` thay vì `New`
- Report Date: 13/08/2026
- Status: Open

## 2. Phân loại
- Severity: **Major** (đề xuất)
- Priority: **P1** (đề xuất)
- Bug Type: Functional / UI
- Component: Interview Schedule / Status Filter
- Affects Version: N/A - cần bổ sung

## 3. Môi trường
- Browser: Chrome
- Environment / Build / OS: N/A - cần bổ sung

## 4. Mô tả lỗi

### Steps to Reproduce
1. Đăng nhập với account admin/IT hoặc interviewer/IT.
2. Vào `Interview Schedule`.
3. Mở combo-box `Status`.
4. Quan sát danh sách Status.
5. Đối chiếu với BRL-16-02 / TC16-07.
6. Thử filter với các status.

### Repro Rate
N/A - chưa được cung cấp số liệu Repro Rate riêng.

### Expected Result
Status dropdown phải chứa:
- New
- Invited
- Interviewed
- Cancelled

### Actual Result
Dropdown hiển thị:
- Open
- Invited
- Interviewed
- Cancelled

`New` bị thiếu và được thay bằng `Open`.

## 5. Evidence
- Screenshot: `screenshot_07(2)`
- Log: Không truy cập được.
- DB / State: Không truy cập được.
- Code Review: tìm thấy `"Open"` hard-code trong Status dropdown.
- Evidence bổ sung: build/version, screenshot/code diff sau fix và regression evidence.

## 6. Root Cause

**Đã VERIFIED.**

> FE khai báo hard-code giá trị/label `Open` thay vì `New` trong danh sách option của Status dropdown.

### Kết quả verify
| Hypothesis | Kết quả | Số liệu |
|---|---|---:|
| H1 | **Confirmed** | Code review: tìm thấy `Open` hard-code |
| H2 | **Rejected** | 2/10 |
| H3 | **Rejected** | 1/10 |

## 7. Trace
- Primary Test Case: **TC16-07**
- Related Test Cases: **TC16-08, TC16-09**
- Requirement: **UC16 REF 5 / BRL-16-02**

## 8. Suggested Fix
1. Sửa FE option `Open` → `New`.
2. Đảm bảo 4 status nghiệp vụ đúng BRL-16-02.
3. Giữ `All` là default filter nếu được requirement quy định.
4. Regression TC16-07, TC16-08, TC16-09.
5. Sau fix ghi nhận Repro Rate X/Y.

---

# BUG 5 – Thiếu action View cho Role C/D

## 1. Thông tin định danh
- Bug ID: `BUG-INTERVIEW-005`
- Title: Thiếu action View cho Role C/D trên Interview Schedule
- Report Date: 13/08/2026
- Status: Open

## 2. Phân loại
- Severity: **Major** (đề xuất)
- Priority: **P1** (đề xuất)
- Bug Type: Functional / UI / Permission Rendering
- Component: Interview Schedule / Action column
- Affects Version: N/A - cần bổ sung

## 3. Môi trường
- Browser: Chrome
- Environment / Build / OS: N/A - cần bổ sung
- Permission/role rendering: Có liên quan

## 4. Mô tả lỗi

### Steps to Reproduce
1. Đăng nhập bằng account Role C – Admin/IT.
2. Vào `Interview Schedule`.
3. Quan sát cột `Action`.
4. Đăng xuất.
5. Đăng nhập bằng account Role D – Interviewer/IT.
6. Vào lại `Interview Schedule`.
7. Quan sát cột `Action`.

### Repro Rate
**19/20** theo kết quả verify H1.

### Expected Result
Icon/action `View` phải hiển thị trên các dòng dữ liệu với **tất cả roles**.

### Actual Result
Với Role C và Role D:
- Không có icon/action `View`.
- Không thể thực hiện View từ UI.

### Coverage limitation
- Role C: Tested
- Role D: Tested
- Role A: Chưa test
- Role B: Chưa test

Không kết luận bug đã được reproduce trên 4/4 role.

## 5. Evidence
- Screenshot: `screenshot_16`
- Log: Không truy cập được.
- Request/Response: Không có.
- DB/State: Không có.
- Requirement: UC16 / REF 14
- Test Case: TC16-16

### Verify results
| Hypothesis | Kết quả | Số liệu |
|---|---|---:|
| H1 | **Confirmed** | 19/20 |
| H2 | **Confirmed** | 18/20 |
| H4 | **Chưa rõ ràng** | 6/10 |

## 6. Root Cause

**Đã VERIFIED.**

> FE áp dụng sai logic permission khi render action View, khiến View bị coi là action có giới hạn role thay vì action available cho tất cả role, đặc biệt với Role C và D.

**Symptom:** Icon View không xuất hiện.

**Root Cause:** FE permission/rendering logic áp dụng role restriction sai cho action View.

## 7. Trace
- Test Case: **TC16-16**
- Requirement: **UC16 / REF 14**

## 8. Suggested Fix
1. Sửa FE permission/rendering rule của action View.
2. Loại bỏ role restriction đang áp dụng cho View.
3. Đảm bảo rule: `View → available for all roles`.
4. Không áp dụng permission của Edit/Submit Result vào View.
5. Regression với đủ Role A/B/C/D.
6. Re-run TC16-16.

---

# Tổng quan 5 bug

| Bug | Nội dung | Root Cause đã verify | Priority đề xuất | TC chính |
|---|---|---|---|---|
| BUG-001 | Thiếu cột Interviewer/Result | FE thiếu table column definition | P1 | TC16-01 |
| BUG-002 | Sai message no-result | FE hard-code/sai message key | P1 | TC16-04 |
| BUG-003 | Thiếu filter Interviewer | FE chưa implement Interviewer component/field | P1 | TC16-05 / TC16-21 |
| BUG-004 | Status Open thay vì New | FE hard-code sai Status option | P1 | TC16-07 |
| BUG-005 | Thiếu View cho Role C/D | FE áp dụng sai permission logic | P1 | TC16-16 |

## Tổng hợp Root Cause theo layer

| Layer | Bug | Root Cause |
|---|---|---|
| **FE** | BUG-001 | Thiếu cấu hình 2 table columns |
| **FE** | BUG-002 | Sai hard-code/message key |
| **FE** | BUG-003 | Chưa implement Interviewer filter |
| **FE** | BUG-004 | Hard-code sai Status option |
| **FE** | BUG-005 | Sai permission rendering logic |

## Kết luận

Cả 5 bug đều đã có **Root Cause được verify ở Frontend** theo evidence và kết quả verification do người dùng cung cấp. Không bug nào trong 5 bug này được ghi Root Cause là `CHƯA VERIFY`.

Các mức **Severity/Priority** trong tài liệu là **đề xuất QA**, không phải giá trị đã được PO/PM phê duyệt.
