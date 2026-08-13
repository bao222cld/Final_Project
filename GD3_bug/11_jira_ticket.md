# Jira Tickets – UC16 Interview Schedule
## Tổng hợp BUG-INTERVIEW-001 → BUG-INTERVIEW-005

---

# BUG-INTERVIEW-001 – Thiếu cột Interviewer và Result

```text
Summary        : [Interview Schedule] Thiếu cột Interviewer và Result
Issue Type     : Bug
Component      : Interview Schedule
Affects Version: N/A - cần bổ sung
Severity       : Major (đề xuất)
Priority       : P1 (đề xuất - chờ người chốt)
Labels         : interview-schedule, ui, missing-column
Linked issues  : N/A
Environment    : Chrome | Environment/Build/OS: N/A - cần bổ sung
```

### Description

**Mô tả**

Bảng Interview Schedule không hiển thị 2 cột `Interviewer` và `Result`, mặc dù UC16 yêu cầu cả hai field phải có trong result table.

**Steps to Reproduce**

1. Đăng nhập với tài khoản có quyền truy cập Interview Schedule.
2. Vào `Interview Schedule`.
3. Quan sát các column của bảng.
4. Đối chiếu với UC16 / REF 10 và REF 11.

**Repro Rate:** N/A - chưa có số liệu repro độc lập.

**Expected**

Bảng phải hiển thị đầy đủ các column theo UC16, bao gồm:
- Interviewer (REF 10)
- Result (REF 11)

TC16-01 yêu cầu đủ 8 cột: Title, Candidate Name, Interviewer, Schedule, Result, Status, Job, Action.

**Actual**

Hai cột `Interviewer` và `Result` không được render trong bảng Interview Schedule.

**Evidence**

- Log: Không có / không truy cập được.
- DB/State: N/A.
- Evidence bổ sung: screenshot hiện trạng và build/version cụ thể nếu cần.

**Root Cause (đã verify)**

FE thiếu cấu hình 2 cột `Interviewer` và `Result` trong component render bảng Interview Schedule; table column definition chưa được cập nhật theo UC16.

Verify:
- H1: Confirmed — 19/20
- H2: Rejected — 3/20
- H3: Rejected — 1/20

**Trace**
- Test Case: TC16-01
- Related Test Case: TC16-17
- Requirement: UC16 / REF 10 / REF 11

---

# BUG-INTERVIEW-002 – Sai message no-result

```text
Summary        : [Interview Schedule] Search không có kết quả hiển thị sai ME008
Issue Type     : Bug
Component      : Interview Schedule
Affects Version: N/A - cần bổ sung
Severity       : Major (đề xuất)
Priority       : P1 (đề xuất - chờ người chốt)
Labels         : interview-schedule, search, ui, message
Linked issues  : N/A
Environment    : Chrome | Environment/Build/OS: N/A - cần bổ sung
```

### Description

**Mô tả**

Khi Search bằng keyword không tồn tại trên màn hình Interview Schedule, hệ thống hiển thị sai message thay vì message ME008.

**Steps to Reproduce**

1. Đăng nhập vào hệ thống.
2. Truy cập `Interview Schedule`.
3. Nhập keyword không tồn tại vào Search box.
4. Click `Search`.
5. Quan sát message khi không có kết quả.

**Repro Rate:** 20/20

**Expected**

Hiển thị đúng ME008:

`No items match with your search data. Please try again`

và bảng kết quả trống.

Requirement: UC16 REF 6 / ME008.

**Actual**

Hiển thị:

`No Interview Schedule has been found`

**Evidence**

- Screenshot: cần bổ sung tên file cụ thể.
- Log: Không truy cập được.
- DB/State: N/A.

**Root Cause (đã verify)**

FE hard-code hoặc sử dụng sai message key cho trạng thái Search không có kết quả, khiến UI hiển thị `No Interview Schedule has been found` thay vì ME008 `No items match with your search data. Please try again`.

Verify:
- H1: Confirmed — 20/20
- H2: Confirmed — 17/20
- H5: Rejected — 1/10

**Trace**
- Test Case: TC16-04
- Requirement: UC16 REF 6 / ME008

---

# BUG-INTERVIEW-003 – Thiếu filter Interviewer

```text
Summary        : [Interview Schedule] Thiếu filter Interviewer
Issue Type     : Bug
Component      : Interview Schedule
Affects Version: N/A - cần bổ sung
Severity       : Major (đề xuất)
Priority       : P1 (đề xuất - chờ người chốt)
Labels         : interview-schedule, filter, ui, interviewer
Linked issues  : N/A
Environment    : Chrome | Environment/Build/OS: N/A - cần bổ sung
```

### Description

**Mô tả**

Màn hình Interview Schedule không hiển thị combo-box `Interviewer` trong Search section, khiến người dùng không thể lọc lịch phỏng vấn theo Interviewer hoặc kết hợp đủ 3 filter Search + Interviewer + Status.

**Steps to Reproduce**

1. Đăng nhập với account admin/IT hoặc interviewer/IT.
2. Truy cập `Interview Schedule`.
3. Quan sát khu vực Search/Filter.
4. Kiểm tra các control filter.

**Repro Rate:** 30/30

**Expected**

Search section phải có combo-box `Interviewer`.

Nếu để trống, hệ thống phải không áp dụng điều kiện filter Interviewer.

Requirement: UC16 REF 4.

**Actual**

Không có combo-box/field `Interviewer`. Chỉ có Search box và Status filter.

**Evidence**

- `screenshot_05`
- `screenshot_06`
- `screenshot_21`
- Log: Không truy cập được.
- DB/State: Không áp dụng cho symptom UI.

**Root Cause (đã verify)**

FE chưa implement component/field `Interviewer` trong Search section của màn hình Interview Schedule.

Verify:
- H1: Confirmed — 30/30
- H2: Rejected — 2/20
- H5: Rejected — 0/10 (ON)

**Trace**
- Test Cases: TC16-05, TC16-06, TC16-21
- Requirement: UC16 REF 4

---

# BUG-INTERVIEW-004 – Status "Open" thay vì "New"

```text
Summary        : [Interview Schedule] Status dropdown hiển thị "Open" thay vì "New"
Issue Type     : Bug
Component      : Interview Schedule / Status Filter
Affects Version: N/A - cần bổ sung
Severity       : Major (đề xuất)
Priority       : P1 (đề xuất - chờ người chốt)
Labels         : interview-schedule, status-filter, ui
Linked issues  : N/A
Environment    : Chrome | Environment/Build/OS: N/A - cần bổ sung
```

### Description

**Mô tả**

Status dropdown trên màn hình Interview Schedule hiển thị `Open` thay vì status chuẩn `New` được quy định trong BRL-16-02.

**Steps to Reproduce**

1. Đăng nhập với account admin/IT hoặc interviewer/IT.
2. Vào `Interview Schedule`.
3. Mở combo-box `Status`.
4. Quan sát danh sách Status.
5. Đối chiếu với BRL-16-02 / TC16-07.

**Repro Rate:** N/A - chưa được cung cấp số liệu repro riêng.

**Expected**

Status dropdown phải có:
- New
- Invited
- Interviewed
- Cancelled

**Actual**

Dropdown hiển thị:
- Open
- Invited
- Interviewed
- Cancelled

`New` bị thiếu và được thay bằng `Open`.

**Evidence**

- `screenshot_07(2)`
- Code Review: tìm thấy `Open` hard-code trong Status dropdown.
- Log: Không truy cập được.
- DB/State: Không truy cập được.

**Root Cause (đã verify)**

FE khai báo hard-code giá trị/label `Open` thay vì `New` trong danh sách option của Status dropdown.

Verify:
- H1: Confirmed — code review tìm thấy `Open` hard-code
- H2: Rejected — 2/10
- H3: Rejected — 1/10

**Trace**
- Test Case: TC16-07
- Related Test Cases: TC16-08, TC16-09
- Requirement: UC16 REF 5 / BRL-16-02

---

# BUG-INTERVIEW-005 – Thiếu action View cho Role C/D

```text
Summary        : [Interview Schedule] Thiếu action View với Role C/D
Issue Type     : Bug
Component      : Interview Schedule / Action column
Affects Version: N/A - cần bổ sung
Severity       : Major (đề xuất)
Priority       : P1 (đề xuất - chờ người chốt)
Labels         : interview-schedule, action, view, permission, ui
Linked issues  : N/A
Environment    : Chrome | Environment/Build/OS: N/A - cần bổ sung
```

### Description

**Mô tả**

Action/Icon `View` không hiển thị trong cột Action của Interview Schedule với Role C – Admin/IT và Role D – Interviewer/IT, mặc dù REF 14 quy định View phải available cho tất cả roles.

**Steps to Reproduce**

1. Đăng nhập bằng account Role C – Admin/IT.
2. Vào `Interview Schedule`.
3. Quan sát cột `Action`.
4. Đăng xuất.
5. Đăng nhập bằng account Role D – Interviewer/IT.
6. Vào lại `Interview Schedule`.
7. Quan sát cột `Action`.

**Repro Rate:** 19/20 theo kết quả verify H1.

**Expected**

Icon/action `View` phải hiển thị trên các dòng dữ liệu của Interview Schedule với tất cả roles.

Requirement: UC16 / REF 14.

**Actual**

Icon/action `View` không hiển thị trong cột Action với Role C và Role D.

Coverage:
- Role C: Tested
- Role D: Tested
- Role A: Chưa test
- Role B: Chưa test

**Evidence**

- `screenshot_16`
- Log: Không truy cập được.
- DB/State: Không có.

**Root Cause (đã verify)**

FE áp dụng sai logic permission khi render action View, khiến View bị coi là action có giới hạn role thay vì action available cho tất cả role, đặc biệt với Role C và Role D.

Verify:
- H1: Confirmed — 19/20
- H2: Confirmed — 18/20
- H4: Chưa rõ ràng — 6/10

**Trace**
- Test Case: TC16-16
- Requirement: UC16 / REF 14

---

# Bảng tổng hợp Jira Tickets

| Bug | Summary | Severity | Priority | Root Cause |
|---|---|---|---|---|
| BUG-001 | Thiếu cột Interviewer và Result | Major | P1 | FE thiếu table column definition |
| BUG-002 | Search no-result hiển thị sai ME008 | Major | P1 | FE hard-code/sai message key |
| BUG-003 | Thiếu filter Interviewer | Major | P1 | FE chưa implement Interviewer component/field |
| BUG-004 | Status Open thay vì New | Major | P1 | FE hard-code sai Status option |
| BUG-005 | Thiếu action View cho Role C/D | Major | P1 | FE áp dụng sai permission rendering logic |

> Tất cả Severity/Priority trên là **đề xuất QA**, chưa phải giá trị được PO/PM phê duyệt.

