# UC16: View Interview Schedule

## 3.16.1 Overview

| Field | Content |
|---|---|
| ID and Name | UC16: View Interview Schedule |
| Description | This use case allows user to view interview schedule list |
| Actor | Role A, Role B, Role C, Role D |
| Trigger | User clicks "Interview" icon on the horizontal left menu bar |
| Pre-condition | System is available |
| Post-condition | System shows interview schedule list |

## 3.16.2 Flow of events

### 3.16.2.1 Basic Flow

| Step | Actor | Action | Next |
|---|---|---|---|
| 1 | Role A / Role B / Role C / Role D | Clicks "Interview" on the horizontal left menu bar | Step 2 |
| 2 | System | Shows interview schedule list | End |

## 3.16.3 Mock-up Screen

- Pic.15 – Interview schedule list for Role B, Role C, Role A
- Pic.25 – Interview schedule list for Role D

URL (masked): `https://app.example.local/candidate-list`

Both screens share the same layout: Search box, Interviewer filter, Status filter, Search button, result table (Title, Candidate Name, Interviewer, Schedule, Result, Status, Job, Action), pagination.

## 3.16.4 Screen Description

### 3.16.4.1 Interview schedule list / Search section

| REF | Field Name | Control Type | Data Type | Description |
|---|---|---|---|---|
| 1 | Name of module | Label | N/A | Show the name of module "Interview Schedule" |
| 2 | Name of sub-module | Label | N/A | Show the name of sub-module "Interview Schedule list" |
| 3 | Search box | Textbox | Text | User input information in the search box to search. Allow to search partial match on all columns in the result table |
| 4 | Interviewer | Combo-box | Text | Allow to select an interviewer to search. If blank, search all interviewer |
| 5 | Status | Combo-box | Text | Allow to select a status to search. Default to all. List of Interview Schedule status: refer to BRL-16-02 |
| 6 | Search Button | Button | N/A | Click to start search. If no data match, show ME008: "No items match with your search data. Please try again" |
| 7 | Add | Icon | N/A | User clicks icon Add, system shows "Create job", only available for Role B, Role A and Role C |

### 3.16.4.2 Interview schedule list

| REF | Field Name | Control Type | Data Type | Description |
|---|---|---|---|---|
| 8 | Interview title | Label | Text | Interview title show the title of schedule |
| 9 | Candidate | Label | Text | Display candidate's name |
| 10 | Interviewer | Label | Text | Display interviewer's name |
| 11 | Result | Label | Text | Display the interview result: Passed or Failed. If no result yet, display as N/A |
| 12 | Schedule | Label | Date time | Display the Schedule time in format: DD/MM/YYYY HH:MM – HH:MM |
| 13 | Status | Label | Text | Display the Interview Status: refer to BRL-16-02 |
| 14 | Icon view | Button | Icon | When click, display view interview details schedule screen. Available for all roles |
| 15 | Icon Edit | Button | Icon | When click, display Edit schedule screen. Not available for Role D |
| 16 | Icon Submit result | Button | Icon | When click, display Submit result screen for interviewer. Not available for Role B, Role A and Role C |

## 3.16.5 Business Rules

| Business Rule ID | Business Rule Description |
|---|---|
| BRL-16-01 | Only user with Role A, Role B and Role C can edit the interview. Role D can view and submit result in schedule details. |
| BRL-16-02 | There're 4 statuses of the Interview Schedule: <br> - New: when the interview schedule is created <br> - Invited: When the reminder is sent to Role D <br> - Interviewed: When Role D has submitted the result for the interview schedule <br> - Cancelled: when the Interview is Cancelled |

> Tài liệu này đã được xử lý theo đúng bảng quy tắc anonymize (xem `masking_glossary.md`): Activity Flow chuyển thành bảng Step\|Actor\|Action\|Next, vai trò được khái quát hoá thành Role A/B/C/D, URL nội bộ đã thay bằng domain giả, không nhúng ảnh mockup hay dữ liệu mẫu chứa tên người thật.