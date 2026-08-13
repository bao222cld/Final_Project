# Bug001

Đã đối chiếu Bug Description + 3 screenshot + UC16 + bộ Test Case. Theo yêu cầu, tôi chỉ thực hiện GIAI ĐOẠN A, chưa đưa bất kỳ giả thuyết nguyên nhân gốc nào.

GIAI ĐOẠN A — PHÂN TÍCH BUG
1. Tóm tắt hiện tượng

Bug nằm tại màn hình Interview Schedule List:

UI thực tế chỉ hiển thị 6 cột:
Interview Title
Candidate Name
Job Title
Schedule
Status
Action
Theo UC16, bảng phải có 8 cột dữ liệu gồm:
Interview Title
Candidate
Interviewer
Result
Schedule
Status
Job
Action.
Interviewer không tồn tại như một cột độc lập.
Result không tồn tại như một cột độc lập; thay vào đó, thông tin Passed/Failed đang xuất hiện bên trong Status dưới dạng Interviewed - Pass/Fail.
Vì vậy, với trường hợp Cancelled, không có cột Result độc lập để xác nhận N/A.

Đây là symptom/hiện tượng quan sát được, chưa phải Root Cause.

2. Timeline sự kiện
Thời điểm	Sự kiện	Evidence
13/08/2026	Thực hiện test trên Chrome	Thông tin test được cung cấp
Bước 1	Đăng nhập bằng account Role C hoặc Role D	Bug description
Bước 2	Click menu Interview	UC16 Basic Flow yêu cầu hệ thống hiển thị Interview Schedule List.
Bước 3	Quan sát bảng Interview Schedule	Screenshot cho thấy bảng thực tế
Bước 4	Đếm số cột	Screenshot cho thấy 6 cột thay vì cấu trúc 8 cột theo UC16
Bước 5	Đối chiếu với expected của TC16-01	TC yêu cầu đủ 8 cột.
Bước 6	Đối chiếu TC16-17	Không tồn tại cột Result để kiểm tra Passed/Failed/N/A.
Bước 7	Đối chiếu TC16-22	Không có cột Result độc lập để xác nhận N/A khi Status = Cancelled.
Điểm đáng chú ý

Screenshot đầu tiên cho thấy rõ Status đang hiển thị dạng:

Interviewed - Pass
Interviewed - Fail

Trong khi specification tách Result và Status thành hai field riêng biệt. UC16 quy định Result là Passed, Failed hoặc N/A, còn Status có 4 giá trị New, Invited, Interviewed, Cancelled.

3. Phân loại bug
Dimension	Đánh giá
Category	Functional / UI
Bug Type	Functional — thiếu/hiển thị sai field trên bảng
Primary Layer	FE / Presentation layer — dựa trên biểu hiện UI
Component	Interview → Interview Schedule List
Data presentation	Sai cấu trúc/cách hiển thị field
Root Cause layer	CHƯA XÁC ĐỊNH
API/BE involvement	Chưa thể kết luận
DB involvement	Chưa thể kết luận
Vì sao chưa kết luận BE/DB?

Hiện chỉ có screenshot của UI. Không có:

API request/response
Network trace
BE log
DB state

Do đó có thể xác định lỗi quan sát được nằm ở output/UI, nhưng chưa thể xác định nguyên nhân nằm ở FE, API mapping, BE response hay DB.

Đặc biệt không được kết luận:

"FE không implement cột Interviewer/Result"

vì hiện chưa có source code hoặc API response để chứng minh điều đó.

4. Expected vs Actual
Expected

Theo UC16, bảng Interview Schedule phải có các field riêng biệt:

Interview title | Candidate | Interviewer | Result | Schedule | Status | Job | Action

Trong đó:

Interviewer: hiển thị tên interviewer.
Result: hiển thị Passed, Failed; nếu chưa có kết quả thì N/A.
Status: chỉ biểu diễn trạng thái Interview Schedule theo BRL-16-02.

TC16-17 tiếp tục yêu cầu phải kiểm tra đủ 3 trạng thái của Result: Passed, Failed, N/A.

TC16-22 yêu cầu riêng:

Khi Status = Cancelled → Result = N/A.

Actual

Evidence screenshot cho thấy bảng chỉ có:

Interview Title | Candidate Name | Job Title | Schedule | Status | Action

Không có:

Interviewer
Result

Ngoài ra, Status đang chứa thông tin dạng:

Interviewed - Pass / Interviewed - Fail

thay vì tách riêng Status = Interviewed và Result = Passed/Failed.

5. Impact
Ảnh hưởng trực tiếp

Bug làm 3 test case Fail/Block:

TC	Priority	Ảnh hưởng
TC16-01	P1	Không thể xác nhận bảng có đủ 8 cột theo specification
TC16-17	P2	Không thể kiểm tra Result = Passed / Failed / N/A qua một cột Result độc lập
TC16-22	P2	Không thể xác nhận Result = N/A khi Status = Cancelled

TC16-01 quy định rõ expected phải có đủ 8 cột.

TC16-17 yêu cầu Result phải biểu diễn riêng Passed, Failed, N/A.

TC16-22 yêu cầu riêng trường hợp Cancelled phải có Result N/A.

Impact nghiệp vụ

Hiện tại người dùng có thể vẫn nhìn thấy danh sách interview, nên chưa có evidence cho thấy toàn bộ chức năng Interview bị unavailable.

Tuy nhiên, cấu trúc dữ liệu trên màn hình không đáp ứng contract UI được định nghĩa trong UC16, dẫn đến:

Không xác định được interviewer trực tiếp trên từng dòng.
Không phân biệt được rõ Status và Result.
Không thể kiểm tra Result N/A cho Cancelled.
Các test case liên quan đến cấu trúc và giá trị Result không thể thực hiện/pass theo expected.
6. Traceability: Bug được phát hiện từ Test Case nào?

Có 3 TC trực tiếp liên quan.

TC16-01 — phát hiện lỗi cấu trúc bảng

Expected yêu cầu:

đủ 8 cột: Title, Candidate Name, Interviewer, Schedule, Result, Status, Job, Action.

Observed: chỉ có 6 cột.

→ Đây là TC trực tiếp phát hiện missing Interviewer + Result.

TC16-17 — phát hiện lỗi Result

Expected yêu cầu:

Passed → Passed
Failed → Failed
Chưa có kết quả → N/A.

Observed: không có cột Result độc lập.

TC16-22 — phát hiện lỗi Cancelled/N/A

Expected:

Status = Cancelled → Result = N/A.

Observed: không có cột Result để xác nhận giá trị N/A.

7. Requirement / Business Rule bị vi phạm
Requirement	Nội dung	Trạng thái
UC16 REF 10	Có cột Interviewer, hiển thị tên interviewer	FAIL
UC16 REF 11	Có cột Result, hiển thị Passed/Failed/N/A	FAIL
UC16 REF 13	Status hiển thị Interview Status theo BRL-16-02	FAIL/không đáp ứng đúng cấu trúc
BRL-16-02	Status chỉ gồm New, Invited, Interviewed, Cancelled	Có dấu hiệu không phù hợp với cách hiển thị hiện tại
TC16-01 Expected	Đủ 8 cột	FAIL
TC16-17 Expected	Result độc lập	FAIL
TC16-22 Expected	Cancelled → Result N/A	BLOCK/FAIL

Đặc biệt, BRL-16-02 định nghĩa Status chỉ có 4 trạng thái: New, Invited, Interviewed, Cancelled.

Do screenshot thể hiện Interviewed - Pass/Fail, cần thận trọng: chưa thể khẳng định dữ liệu backend thực sự có Status = "Interviewed - Pass". Có khả năng đây chỉ là cách UI render. Vì vậy đây là evidence về presentation, chưa phải bằng chứng về giá trị state bên dưới.

8. Severity / Priority — đề xuất
Severity: Major — đề xuất

Lý do:

Chức năng chính vẫn mở được.
Danh sách interview vẫn hiển thị.
Không có evidence cho thấy hệ thống crash hoặc mất dữ liệu.
Nhưng UI thiếu 2 field được specification yêu cầu và làm 3 TC không thể đạt expected.
Có ảnh hưởng đến khả năng quan sát/kiểm chứng dữ liệu nghiệp vụ.

Theo định nghĩa trong Format Bug Report, Major là lỗi chức năng rõ ràng nhưng chức năng vẫn còn sử dụng được.

Priority: P1 — đề xuất

Lý do:

Có 3 TC bị ảnh hưởng, trong đó TC16-01 là P1.
Lỗi nằm ngay trên màn hình danh sách chính của UC16.
Việc thiếu field làm mất khả năng verify các requirement về Interviewer/Result.

Lưu ý: Priority cuối cùng vẫn cần người/PO/PM chốt; đây chỉ là đề xuất QA theo evidence hiện tại.

9. Evidence hiện có
Evidence	Trạng thái	Nhận xét
Screenshot 01	✅ Có	Hiển thị toàn bộ Interview Schedule List và 6 header
Screenshot 17	✅ Có	Xác nhận một row chỉ có cấu trúc 6 cột; Status chứa Interviewed - Fail
Screenshot 22	✅ Có	Xác nhận header của bảng chỉ có 6 cột
Bug description	✅ Có	Mô tả expected/actual và TC liên quan
UC16	✅ Có	Có specification chính thức cho 8 field
Test cases	✅ Có	Có TC16-01, TC16-17, TC16-22
Browser	✅ Có	Chrome
Test date	✅ Có	13/08/2026
Log	❌ Không có	Không truy cập được
API Request/Response	❌ Không có	Chưa thu thập
DB State	❌ Không có	Không truy cập được
Build/version	❌ Không có	Chưa cung cấp
OS/device	❌ Không có	Chưa cung cấp
Test data/state cụ thể	⚠️ Thiếu	Chưa có dữ liệu backend để xác nhận Interviewer/Result thực tế
10. Evidence còn thiếu — ưu tiên thu thập

Đây là phần quan trọng để không nhầm symptom với root cause.

P0 — cần nhất

1. API Request/Response của Interview Schedule

Cần kiểm tra response có chứa:

interviewer
result
status
job

hay không.

Mục tiêu: phân biệt:

API đã trả Interviewer/Result nhưng FE không render
hay API không trả các field này.

2. Network trace / DevTools

Capture request khi mở màn hình Interview Schedule.

Cần có:

Request URL
HTTP method
Response status
Response JSON
P1

3. DB/state của một record Interview

Đặc biệt cần ít nhất các state:

Interviewed + Passed
Interviewed + Failed
Cancelled

Mục tiêu là xác nhận:

Status và Result thực tế có tồn tại độc lập hay không.

4. Screenshot/record của dòng Cancelled

Bug description nói TC16-22 kiểm tra Cancelled, nhưng evidence screenshot hiện tại chưa cho thấy rõ một row Cancelled.

Vì vậy hiện chỉ có thể nói:

Không có cột Result trên UI nên không thể quan sát Result = N/A.

Chưa thể dùng screenshot hiện tại để chứng minh giá trị backend của row Cancelled.

P2

5. Build/version

Hiện chỉ biết:

Chrome | 13/08/2026

Chưa biết build frontend/backend đang test.

6. OS/device

Cần bổ sung để hoàn thiện environment.

11. Kết luận Giai đoạn A

Đã xác nhận bằng evidence:

Interview Schedule List đang hiển thị 6 cột thay vì cấu trúc 8 cột theo UC16, trong đó Interviewer và Result bị thiếu khỏi bảng. Thông tin Passed/Failed hiện quan sát được nằm trong phần Status dưới dạng Interviewed - Pass/Fail. Điều này vi phạm cấu trúc UI được định nghĩa tại UC16 và khiến TC16-01, TC16-17 và TC16-22 không đạt expected.

Chưa được xác nhận:

Chưa thể xác định Root Cause nằm ở FE, API/BE mapping hay DB/state vì hiện không có API response, log hoặc DB evidence.

Vì vậy tại thời điểm này:

Symptom: Missing Interviewer + Result columns / Result bị gộp trong presentation của Status.
Root Cause: CHƯA VERIFY — không được kết luận ở Giai đoạn A.

Đây là điểm dừng phù hợp của Giai đoạn A theo yêu cầu; chưa đưa bảng giả thuyết nguyên nhân gốc.

---

# Bug002

Đã đối chiếu screenshot_04, UC16 và bộ Test Case. Theo yêu cầu, dưới đây chỉ là GIAI ĐOẠN A, chưa đưa giả thuyết nguyên nhân gốc.

GIAI ĐOẠN A — PHÂN TÍCH BUG
1. Tóm tắt hiện tượng

Khi người dùng nhập một từ khóa không khớp dữ liệu trong Interview Schedule và bấm Search, hệ thống không hiển thị message chuẩn ME008.

Actual trên screenshot
Search box: zzxxxx123
Kết quả: 0/0 rows
Message hiển thị:

"No Interview Schedule has been found"

Expected

Theo UC16 REF 6, khi Search không có dữ liệu match, hệ thống phải hiển thị ME008:

"No items match with your search data. Please try again"

TC16-04 cũng yêu cầu đúng nguyên văn message này và bảng kết quả phải trống.

Kết luận symptom: hệ thống xử lý trường hợp no-result, nhưng nội dung message hiển thị không đúng message chuẩn ME008.

2. Timeline sự kiện
Bước	Sự kiện	Evidence
1	Đăng nhập với account Admin/IT hoặc Interviewer/IT	Bug description
2	Truy cập menu Interview	Phù hợp UC16 Basic Flow: user click Interview → hệ thống hiển thị Interview Schedule List.
3	Nhập từ khóa không tồn tại vào Search box	Screenshot: zzxxxx123
4	Click Search	Bug description + screenshot
5	Hệ thống không trả về record	Screenshot: 0/0 rows
6	Hệ thống hiển thị message	Screenshot: No Interview Schedule has been found
7	Đối chiếu requirement	Expected phải là ME008: No items match with your search data. Please try again.
8	Đối chiếu TC16-04	TC yêu cầu message đúng nguyên văn ME008.
3. Phân loại bug
Dimension	Đánh giá
Category	Functional / UI
Bug Type	Functional
Component	Interview → Interview Schedule → Search
Layer quan sát được	FE / Presentation
Nature	Incorrect static/message content
Data issue	Không có evidence cho thấy dữ liệu bị sai
API issue	Chưa xác định
DB issue	Không áp dụng theo evidence hiện tại
Root Cause	CHƯA XÁC ĐỊNH
Lưu ý về layer

Có thể nói symptom xuất hiện ở UI, nhưng chưa được phép kết luận:

"FE hard-code sai message"

vì chưa có source code hoặc API response.

Message sai có thể là kết quả của nhiều tầng khác nhau; hiện evidence chỉ chứng minh message cuối cùng render trên UI không đúng requirement.

4. Expected vs Actual
Expected Result

Khi Search không tìm thấy dữ liệu:

No items match with your search data. Please try again

và bảng kết quả phải trống. Đây là expected của TC16-04, nguồn REF 6.

UC16 cũng quy định Search Button phải hiển thị ME008 khi không có data match.

Actual Result

Screenshot cho thấy:

Search keyword: zzxxxx123
Kết quả: 0/0 rows
Message:

No Interview Schedule has been found

Do đó:

Result set: phù hợp với trường hợp no-result.
Message content: FAIL.

Đây là điểm quan trọng: bug không phải "Search không hoạt động". Search đã đưa UI về trạng thái không có kết quả; lỗi nằm ở message được hiển thị cho trạng thái đó.

5. Phân biệt Symptom và Root Cause
Symptom đã xác định

Khi search không có dữ liệu, UI hiển thị No Interview Schedule has been found thay vì message chuẩn ME008.

Root Cause

CHƯA VERIFY.

Hiện chưa có evidence để xác định message sai bắt nguồn từ:

FE hard-coded text;
message/config resource;
mapping sai giữa no-result state và message;
API trả message khác;
hoặc một cơ chế khác.

Không được ghi Root Cause = FE hard-code sai message ở thời điểm này.

6. Impact
Functional impact

Chức năng Search vẫn thực hiện được việc trả về 0 record.

Screenshot cho thấy:

0/0 rows

và trạng thái no-result được hiển thị.

Do đó:

Không có evidence về crash.
Không có evidence về mất dữ liệu.
Không có evidence rằng Search trả về dữ liệu sai.
Không có evidence về DB corruption.
Tuy nhiên

Message không tuân thủ requirement chuẩn.

Điều này ảnh hưởng đến:

Consistency của hệ thống message
Khả năng kiểm thử theo expected chính thức
Compliance với ME008/REF 6
TC16-04 không thể PASS.
7. Mức độ ảnh hưởng
Severity — Minor (đề xuất)

Theo định nghĩa trong tài liệu chuẩn, Minor phù hợp với lỗi ảnh hưởng nhỏ đến chức năng nhưng không làm chức năng chính mất khả năng sử dụng.

Lý do:

Search vẫn hoạt động.
Hệ thống vẫn xác định được 0 kết quả.
Không có mất/sai dữ liệu.
Không có crash.
Lỗi chủ yếu nằm ở nội dung thông báo.

Không nên nâng lên Major/Critical chỉ vì TC bị Fail; Severity đánh giá technical impact, không phải priority.

Priority — P1 (đề xuất)

TC16-04 được xác định P1.

Vì đây là message chuẩn được requirement quy định rõ và TC yêu cầu đúng nguyên văn, đề xuất giữ P1 để fix trong sprint hiện tại.

Severity: Minor
Priority: P1 (đề xuất, chờ người chốt)

Hai giá trị này không nên gộp; tài liệu chuẩn cũng yêu cầu phân biệt Severity và Priority.

8. Traceability
Item	Giá trị
Test Case phát hiện	TC16-04
TC Priority	P1
Requirement	UC16 – REF 6
Expected message	ME008: No items match with your search data. Please try again
Actual message	No Interview Schedule has been found
TC Expected	Message đúng nguyên văn ME008 + bảng trống
TC Result	FAIL

TC16-04 được thiết kế chính xác cho trường hợp Search không có kết quả và yêu cầu message ME008.

9. Evidence hiện có
Evidence	Trạng thái	Nội dung
Screenshot_04	✅ Có	Search zzxxxx123, 0/0 rows, message actual
UC16	✅ Có	REF 6 quy định ME008
TC16-04	✅ Có	Expected message chính xác
Browser	✅ Có	Chrome
Ngày test	✅ Có	13/08/2026
Log	❌ Không có	Không truy cập được
API Request/Response	❌ Không có	Chưa thu thập
DB/State	N/A	Không cần thiết để chứng minh symptom message
Build version	❌ Không có	Chưa cung cấp
OS/Device	❌ Không có	Chưa cung cấp
Message source/config	❌ Không có	Chưa biết message được lấy từ đâu
10. Repro Rate

Hiện evidence chỉ cung cấp một lần quan sát qua screenshot.

Vì vậy không được tự ghi 10/10, 3/3...

Có thể ghi ở mức evidence hiện tại:

Observed: 1 lần / 1 lần được cung cấp evidence

Nhưng đây chưa phải repro rate chính thức, vì chưa có dữ liệu về số lần test thực tế.

Nếu cần chuẩn hóa theo format Bug Report, cần chạy lại test, ví dụ:

cùng account;
cùng keyword không tồn tại;
thực hiện 10 lần;
ghi số lần message sai.
11. Evidence còn thiếu
Cần bổ sung

1. Reproduction count

Cần xác định chính xác:

X/Y lần search no-result cho ra message sai.

2. API Request/Response — nếu có

Mục đích không phải để chứng minh DB mà để xác định message có đến từ backend hay không.

3. Build/version

Hiện chỉ có Chrome + ngày test, chưa có build frontend/backend.

4. OS/Device

Chưa được cung cấp.

5. Message source

Nếu có thể truy cập source/config, cần xác định message:

No Interview Schedule has been found

được định nghĩa ở đâu.

Đây là evidence quan trọng cho Root Cause, nhưng hiện chưa có.

12. Kết luận Giai đoạn A
Đã xác nhận

Khi thực hiện Search với từ khóa không match dữ liệu, hệ thống trả về trạng thái không có kết quả (0/0 rows) nhưng hiển thị No Interview Schedule has been found, không đúng message chuẩn ME008: No items match with your search data. Please try again được quy định tại UC16 REF 6 và TC16-04.

Phân loại

Functional/UI → Interview Schedule → Search → Incorrect no-result message

Impact
Search vẫn sử dụng được.
Không có evidence về lỗi dữ liệu.
Không có evidence về crash.
TC16-04 P1 bị FAIL.
Đánh giá đề xuất

Severity: Minor
Priority: P1 — đề xuất

Root Cause

CHƯA VERIFY.

Hiện tại chỉ đủ bằng chứng để kết luận message actual không khớp requirement, chưa đủ bằng chứng để xác định tại sao message đó được sinh ra.

---

# Bug003

GIAI ĐOẠN A — Phân tích Bug: Thiếu combo-box Interviewer
1. Timeline sự kiện
Thứ tự	Sự kiện	Evidence
1	User đăng nhập bằng account admin/IT hoặc interviewer/IT	Steps to reproduce do QA cung cấp
2	User truy cập menu Interview → Interview Schedule	Screenshot thực tế
3	Màn hình hiển thị khu vực filter gồm Search box + Status dropdown + Search button	Screenshot thực tế
4	Khi mở Status dropdown, các trạng thái Open/Invited/Interviewed/Cancelled được hiển thị	Screenshot thực tế
5	Không có control Interviewer trong khu vực filter	Screenshot thực tế
6	Theo UC16, REF 4 yêu cầu Interviewer = Combo-box, cho phép chọn interviewer; để trống thì tìm tất cả interviewer.	Requirement
7	TC16-05 yêu cầu chọn một interviewer rồi Search để chỉ hiển thị các dòng tương ứng.	Test Case
8	TC16-06 yêu cầu để Interviewer trống và Search để trả về toàn bộ lịch.	Test Case
9	TC16-21 yêu cầu kết hợp Search + Interviewer + Status theo logic AND.	Test Case
10	Do control Interviewer không tồn tại trên UI, TC16-05, TC16-06 và TC16-21 không thể thực hiện đầy đủ.	QA observation
2. Phân loại bug
Category

Functional UI / Missing UI Control

Không chỉ là lỗi visual/layout. Control Interviewer là một thành phần chức năng dùng để lọc dữ liệu, được định nghĩa rõ trong requirement.

Layer

FE / Presentation layer — xác định theo evidence hiện tại

Lý do:

Evidence trực tiếp là giao diện không render combo-box Interviewer.
Search box và Status dropdown vẫn xuất hiện và hoạt động.
Requirement xác định Interviewer là một control trên Search section.
Chưa có evidence cho thấy API/DB bị lỗi.

Tuy nhiên, chưa thể kết luận nguyên nhân nằm ở FE code. Việc "FE không render control" là mô tả layer của symptom hiện tại, không phải Root Cause.

Type

Missing Feature / Missing UI Control

Cụ thể:

Requirement yêu cầu 3 filter chính: Search + Interviewer + Status, nhưng implementation hiện tại chỉ cung cấp Search + Status.

3. Expected vs Actual
Expected

Search section phải có:

Search box
Interviewer combo-box
Status combo-box
Search button

UC16 REF 4 quy định Interviewer là combo-box, cho phép chọn interviewer; nếu để trống thì tìm kiếm trên tất cả interviewer.

Ngoài ra, bảng kết quả cũng phải có field Interviewer riêng để hiển thị tên người phỏng vấn.

Actual

UI hiện tại chỉ quan sát được:

Search box + Status dropdown + Search button

Không có combo-box Interviewer.

Screenshot cũng cho thấy khi mở dropdown hiện tại, đó là Status, với các lựa chọn Open, Invited, Interviewed, Cancelled; không xuất hiện một control Interviewer riêng.

4. Impact
Ảnh hưởng trực tiếp
Test Case	Priority	Impact
TC16-05	P1	Blocked — không thể chọn interviewer để kiểm tra filter
TC16-06	P2	Blocked — không thể kiểm tra trạng thái Interviewer để trống
TC16-21	P1	Blocked — không thể thực hiện đủ 3 điều kiện Search + Interviewer + Status

TC16-05 và TC16-06 đều phụ thuộc trực tiếp vào combo-box Interviewer.

TC16-21 còn yêu cầu logic AND giữa cả Search, Interviewer và Status, nên việc thiếu Interviewer khiến toàn bộ scenario 3-filter không thể thực thi.

Mức độ ảnh hưởng nghiệp vụ

Bug này không làm toàn bộ màn hình Interview Schedule unusable:

Danh sách Interview Schedule vẫn hiển thị.
Search box vẫn tồn tại.
Status filter vẫn tồn tại.
Theo evidence được cung cấp, Search + Status kết hợp vẫn hoạt động.

Nhưng một chức năng filtering được requirement quy định bị thiếu hoàn toàn.

Severity đề xuất

Major

Lý do: chức năng chính vẫn sử dụng được nhưng một filter chức năng bắt buộc bị thiếu, đồng thời block 2 test P1/P2 và 1 test P1.

Priority đề xuất

P1

Lý do:

TC16-05: P1
TC16-21: P1
Đây là requirement rõ ràng trong REF 4, không phải enhancement.
Việc thiếu control khiến không thể hoàn thành coverage của nhóm filtering.

Severity/Priority trên là đề xuất QA, chưa phải mức được người có thẩm quyền chốt.

5. Traceability
Requirement

UC16 — View Interview Schedule

REF 4:

Interviewer | Combo-box | Text | Allow to select an interviewer to search. If blank, search all interviewer

Test Cases bị ảnh hưởng

TC16-05

Combo-box Interviewer lọc đúng danh sách theo interviewer được chọn.

TC16-06

Combo-box Interviewer để trống trả về toàn bộ lịch phỏng vấn.

TC16-21

Kết hợp Search + Interviewer + Status theo logic AND.

6. Phân biệt Symptom và Root Cause
Symptom đã quan sát được

Combo-box Interviewer không xuất hiện trên màn hình Interview Schedule.

Đây không phải Root Cause.

Root Cause

CHƯA VERIFY.

Hiện chưa có evidence để xác định control bị thiếu do:

FE component chưa được implement;
component đã implement nhưng không được render;
sai conditional rendering;
sai configuration/feature flag;
API không cung cấp dữ liệu interviewer;
hoặc nguyên nhân khác.

Do đó không được ghi bất kỳ nguyên nhân nào ở trên thành Root Cause đã xác nhận.

7. Evidence hiện có
Evidence ủng hộ bug
Screenshot UI thực tế: chỉ có Search và Status filter.
UC16 REF 4 quy định phải có Interviewer combo-box.
TC16-05 phụ thuộc trực tiếp vào Interviewer combo-box.
TC16-06 phụ thuộc trực tiếp vào Interviewer combo-box.
TC16-21 yêu cầu đồng thời cả 3 filter.
Evidence hiện chưa có
Không có application log.
Không có browser console log.
Không có Network/API request/response.
Không có source code/component FE.
Không có DOM inspection.
Không có configuration/feature flag.
Không có DB evidence — và DB không phải evidence cần thiết để chứng minh việc control có được render hay không.
Chưa có bằng chứng xác định danh sách interviewer có được API trả về hay không.
Chưa có evidence về repro rate X/Y.
8. Kết luận Giai đoạn A

Classification: Functional UI / Missing UI Control
Layer: FE / Presentation (theo symptom quan sát được, chưa phải root cause)
Type: Missing Feature / Missing UI Control
Severity đề xuất: Major
Priority đề xuất: P1

Confirmed finding:

Màn hình Interview Schedule hiện không hiển thị combo-box Interviewer, trong khi UC16 REF 4 yêu cầu control này. Việc thiếu control trực tiếp khiến TC16-05, TC16-06 và TC16-21 bị Blocked, trong đó TC16-05 và TC16-21 là P1.

Root Cause: CHƯA VERIFY.

Evidence tiếp theo có giá trị nhất để điều tra nguyên nhân là DOM + Network/API response: cần xác định combo-box không tồn tại vì FE không render, hay FE có render logic nhưng dữ liệu/configuration từ phía sau không đủ.

---

# Bug004

GIAI ĐOẠN A — Phân tích bug: Status dropdown sai giá trị
1. Tóm tắt hiện tượng

Symptom quan sát được:

Dropdown Status trên màn hình Interview Schedule đang hiển thị:

Open | Invited | Interviewed | Cancelled

Trong khi đặc tả UC16 quy định 4 trạng thái phải là:

New | Invited | Interviewed | Cancelled

BRL-16-02 xác định New là trạng thái của interview schedule ngay khi schedule được tạo.

Ảnh chụp cung cấp cho thấy trực tiếp dropdown đang có Open thay cho New.

Điểm quan trọng: hiện tại chưa thể kết luận Open xuất phát từ FE hay BE. Với evidence hiện có, chưa biết:

FE hard-code label Open;
FE mapping một enum backend thành Open;
BE/API trả về enum/giá trị Open;
hoặc có một mapping trung gian chuyển New → Open.

Do đó "FE hard-code sai label" chưa phải root cause đã xác nhận.

2. Đối chiếu Requirement / Specification

UC16 quy định Status filter tại REF 5, danh sách status phải tham chiếu BRL-16-02.

BRL-16-02 quy định chính xác 4 status:

Status chuẩn	Ý nghĩa
New	Interview schedule vừa được tạo
Invited	Reminder đã được gửi cho Role D
Interviewed	Role D đã submit result
Cancelled	Interview đã bị cancelled

Test case TC16-07 cũng yêu cầu Status combo-box phải filter được đủ 4 trạng thái: New, Invited, Interviewed, Cancelled.

Ngoài ra:

TC16-08: mặc định phải là All.
TC16-09: status ngoài 4 giá trị hợp lệ được xem là status không hợp lệ.
Kết luận đối chiếu
Tiêu chí	Expected	Actual	Kết quả
Số status	4	4	Đúng
New	Có	Không có	FAIL
Invited	Có	Có	PASS theo evidence test
Interviewed	Có	Có	PASS theo evidence test
Cancelled	Có	Có	PASS theo evidence test
Open	Không thuộc BRL-16-02	Có	FAIL
3. Timeline
Thời điểm	Sự kiện
Trước khi test	UC16/BRL-16-02 quy định 4 status: New, Invited, Interviewed, Cancelled
13/08/2026	QA đăng nhập bằng account admin/IT hoặc interviewer/IT
Sau khi vào Interview Schedule	Mở Status dropdown
Khi quan sát dropdown	Thấy Open, Invited, Interviewed, Cancelled
Sau khi kiểm tra filter	Evidence được cung cấp cho biết 3 status Invited/Interviewed/Cancelled filter đúng; New không thể test do không tồn tại
Hiện tại	Chưa có log/API/DB để xác định nguồn gốc của Open
4. Phân loại bug
Dimension	Đánh giá
Category	Functional / UI
Layer chính	Chưa xác định giữa FE và BE
Layer quan sát được	FE/UI — vì lỗi biểu hiện trực tiếp ở dropdown
Bug type	Functional
Component	Interview Schedule → Status Filter
Sub-type	Incorrect enum/label mapping hoặc incorrect option definition — chưa xác định nguyên nhân
Data dependency	Chưa xác định hoàn toàn; DB không truy cập được
Environment	Chrome
Ngày test	13/08/2026
Lưu ý về layer

Không nên ghi thẳng:

Layer = Frontend

vì evidence hiện tại chỉ chứng minh UI hiển thị sai, chưa chứng minh FE là nơi phát sinh sai.

Cách ghi chính xác hơn:

Observed layer: FE/UI; root layer: FE hoặc BE — cần API evidence để phân định.

5. Impact Analysis
Functional impact

Bug này không làm mất toàn bộ cơ chế filter.

Evidence cho thấy:

Invited filter đúng;
Interviewed filter đúng;
Cancelled filter đúng;
chỉ status New bị thiếu/thay thế bằng Open.

Điều này cho thấy 3/4 giá trị status đang hoạt động theo expected, nhưng bộ filter không đáp ứng đầy đủ requirement.

Ảnh hưởng trực tiếp

Không thể thực hiện đúng việc:

lọc Interview Schedule có status New.

Đặc biệt TC16-07 yêu cầu lần lượt test cả 4 status.

Ngoài ra, nếu hệ thống thực sự sử dụng Open như một status khác với New, việc filter bằng Open có thể làm QA/user truy vấn sai tập dữ liệu. Tuy nhiên chưa có evidence API/DB để xác định semantic của Open.

Test coverage bị ảnh hưởng

TC16-07 — P1

Combo-box Status phải filter đúng 4 trạng thái New, Invited, Interviewed, Cancelled.

TC16-08 — P2

Có liên quan tới tập status của dropdown và default All, nhưng evidence hiện tại chưa cho thấy default All bị lỗi.

TC16-09 — P1

Có liên quan đến việc Open có phải status hợp lệ hay không. Theo requirement, chỉ 4 status của BRL-16-02 là hợp lệ. Tuy nhiên cần kiểm tra API/DB để biết Open chỉ là label hay thực sự được gửi xuống như một status.

6. Severity / Priority — đề xuất
Severity: Major — đề xuất

Lý do:

Đây là lỗi chức năng của filter, không chỉ là lỗi cosmetic.
Một status hợp lệ (New) không thể được lựa chọn/filter.
TC16-07 P1 không thể hoàn thành đúng requirement.
Tuy nhiên 3 status còn lại vẫn hoạt động và màn hình Interview Schedule vẫn sử dụng được.

Theo format Bug Report, Major là lỗi chức năng rõ ràng nhưng vẫn có workaround.

Priority: P1 — đề xuất

Lý do:

TC16-07 là P1.
Lỗi trực tiếp làm mất khả năng kiểm thử một nhánh nghiệp vụ của Status filter.
Requirement chỉ định rõ 4 status.

Severity: Major — đề xuất
Priority: P1 — đề xuất, chờ PO/PM chốt

Severity và Priority cần giữ riêng biệt.

7. Evidence hiện có
Evidence ủng hộ bug
Screenshot screenshot_07(2)
Dropdown hiển thị Open.
Không có New.
Có Invited, Interviewed, Cancelled.
UC16 / BRL-16-02
Requirement xác định đúng 4 status: New, Invited, Interviewed, Cancelled.
TC16-07
Expected Result yêu cầu filter đủ 4 status.
Evidence test được cung cấp:
Invited → filter đúng.
Interviewed → filter đúng.
Cancelled → filter đúng.
New → không thể test vì không tồn tại trong dropdown.
Evidence chưa có
THIẾU EVIDENCE: API Request/Response khi load Status dropdown.
THIẾU EVIDENCE: network payload xác định backend trả New hay Open.
THIẾU EVIDENCE: FE source/config để kiểm tra danh sách enum/label được hard-code.
THIẾU EVIDENCE: DB/state để xác định dữ liệu status thực tế đang lưu là New hay Open.
THIẾU EVIDENCE: log của frontend/backend.
THIẾU EVIDENCE định lượng về Repro Rate X/Y. Hiện chỉ có một lần quan sát được cung cấp, nên không được tự ghi 1/1 nếu chưa thực sự chạy và ghi nhận theo protocol.
8. Phân biệt Symptom và Root Cause
Đã xác nhận

Symptom:

Status dropdown hiển thị Open thay vì New.

Requirement violation:

Bộ status không khớp BRL-16-02/TC16-07.

Chưa xác nhận

Chưa thể nói:

"Frontend hard-code sai."

Cũng chưa thể nói:

"Backend trả sai enum."

Và chưa thể nói:

"Database lưu Open."

Hiện tại chỉ có thể xác nhận lỗi ở mức behavior/UI, còn nguồn gốc kỹ thuật chưa được verify.

9. Điểm cần kiểm chứng tiếp theo

Để phân định chính xác FE hay BE, thứ tự kiểm tra nên là:

UI → Network/API → FE mapping → DB

Cụ thể:

Mở DevTools → Network.
Load Interview Schedule.
Xác định request lấy danh sách/status.
Kiểm tra response:
Nếu response chứa New nhưng UI hiển thị Open → nghiêng về FE mapping/label.
Nếu response đã chứa Open → cần truy tiếp BE/DB.
Nếu response chứa enum code như NEW → kiểm tra FE mapping NEW → Open.
Nếu API trả OPEN → kiểm tra BE enum/mapping và DB.
Sau khi xác định tầng nghi vấn, chạy lại TC16-07 cho đủ 4 status và ghi nhận kết quả định lượng.

Chưa thực hiện các bước trên thì chưa được kết luận Root Cause.

Trace chính
TC16-07 — P1: Status filter phải hỗ trợ New/Invited/Interviewed/Cancelled.
TC16-08 — P2: Default Status phải là All.
TC16-09 — P1: Status ngoài tập hợp hợp lệ phải được xử lý như invalid.
Requirement: REF 5 + BRL-16-02.

Kết luận Giai đoạn A: Bug được xác nhận ở behavior: New bị thay bằng Open trong Status dropdown. Mức Major/P1 là đề xuất. Root Cause: CHƯA VERIFY; evidence hiện tại chưa đủ để phân biệt FE mapping, BE enum/mapping hay DB data.

---

# Bug005

GIAI ĐOẠN A — Phân tích bug: Icon "View" không hiển thị
1. Timeline sự kiện
Thời điểm	Sự kiện	Kết quả
13/08/2026	Đăng nhập bằng account Role C – Admin/IT	Truy cập được Interview Schedule
13/08/2026	Đăng nhập bằng account Role D – Interviewer/IT	Truy cập được Interview Schedule
Sau khi vào màn hình	Quan sát cột Action của các dòng dữ liệu	Không thấy icon View
Sau khi đối chiếu requirement	So sánh với REF 14	Expected: icon View phải available cho tất cả role
Sau khi đối chiếu TC	So sánh với TC16-16	TC16-16 yêu cầu View hiển thị với mọi role và được xếp P1

Lưu ý: Evidence hiện chỉ xác nhận trực tiếp trên 2/4 role: Role C và Role D. Chưa đủ evidence thực nghiệm để kết luận Role A/B cũng bị lỗi.

2. Phân loại bug
Tiêu chí	Phân loại
Category	Functional UI / Access Control – UI rendering
Layer nghi ngờ	FE là layer nghi ngờ chính
Type	Missing UI element / Permission-based rendering
Module	Interview Schedule
Component	Action column / View action
Requirement	REF 14
Test Case	TC16-16 – Icon "View" hiển thị cho tất cả các vai trò
Priority đề xuất	P1 – chờ người chốt
Severity đề xuất	Major – chờ người chốt

Requirement quy định REF 14: icon View là button và available for all roles.

TC16-16 cũng xác định expected result là icon View phải xuất hiện ở mọi dòng, với mọi role, không phụ thuộc phân quyền; TC này được đánh giá P1.

Vì sao chưa xếp Blocker?

Danh sách Interview Schedule vẫn hiển thị được và các action khác vẫn xuất hiện tùy role. Evidence chưa chứng minh toàn bộ module bị unusable hoặc không có bất kỳ workaround nào.

Tuy nhiên, chức năng View interview details bị mất khỏi UI, ảnh hưởng trực tiếp đến một chức năng được requirement quy định cho tất cả role → Major là mức đề xuất hợp lý, chưa phải kết luận severity cuối cùng.

3. Impact
Người dùng bị ảnh hưởng

Đã xác nhận:

Role C – Admin
Role D – Interviewer

Chưa xác nhận:

Role A – Recruiter
Role B – Manager

Vì REF 14 yêu cầu View available cho tất cả role, phạm vi business expectation là 4/4 role, nhưng phạm vi thực nghiệm hiện tại mới 2/4 role.

Functional impact

Người dùng không nhìn thấy action View tại cột Action nên không thể thực hiện thao tác View theo cách UI được đặc tả.

Điều này vi phạm trực tiếp TC16-16.

Không nên kết luận

Chưa có evidence cho thấy:

API view details bị lỗi.
Endpoint view details không tồn tại.
Permission backend từ chối request.
User thực sự không có quyền view.
Click trực tiếp vào một URL/details route có hoạt động hay không.

Do đó, "View không hiển thị" là symptom, chưa thể gọi là root cause.

4. Evidence hiện có
Evidence 1 — Screenshot

Screenshot cho thấy màn hình Interview Schedule có cột Action, nhưng icon View không xuất hiện theo mô tả/evidence đã cung cấp.

Đây là evidence trực tiếp cho UI symptom.

Evidence 2 — Requirement

REF 14 quy định:

Icon View — Available for all roles.

Evidence 3 — Test Case

TC16-16 quy định:

Test với Role bất kỳ.
View phải hiển thị ở cột Action của mọi dòng.
Không phụ thuộc phân quyền.
Priority P1.
Evidence 4 — Các action khác

UC16 phân biệt rõ:

View: tất cả role.
Edit: không available cho Role D.
Submit result: dành cho Role D và không dành cho A/B/C.

Điểm này quan trọng vì nó cho thấy permission-based rendering có tồn tại trong module, nhưng requirement của View lại đặc biệt yêu cầu không giới hạn role.

5. Coverage hiện tại
Role	Đã test	View expected	Actual	Kết quả
Role A – Recruiter	❌	Có	Chưa biết	Coverage gap
Role B – Manager	❌	Có	Chưa biết	Coverage gap
Role C – Admin	✅	Có	Không hiển thị	FAIL
Role D – Interviewer	✅	Có	Không hiển thị	FAIL

Vì vậy có thể ghi chính xác:

Đã reproduce trên 2/2 role được cấp quyền test; chưa đủ evidence để kết luận 4/4 role đều bị ảnh hưởng.

6. Evidence còn thiếu

Đây là phần quan trọng nhất trước khi chuyển sang RCA.

Thiếu evidence về FE
DOM/HTML của cột Action để xác định:
View element không được render, hay
View element có render nhưng bị CSS/visibility che.
Browser DevTools Console.
Network request khi load Interview Schedule.
Frontend code/config liên quan đến permission của action View.
Thiếu evidence về BE/API

Chưa biết API trả về dữ liệu/action permission như thế nào.

Cần kiểm tra:

Response API của Interview Schedule.
Có field kiểu canView, permissions, actions hoặc tương đương hay không.
Backend có trả permission View cho Role C/D hay không.
Thiếu evidence về authorization

Cần xác định:

Backend có cho phép Role C/D View nhưng FE không render, hay backend đã trả permission sai?

Hiện tại chưa thể phân biệt hai trường hợp này.

Thiếu coverage

Cần test thêm:

Role A
Role B

để xác định phạm vi ảnh hưởng thực tế.

7. Kết luận Giai đoạn A

Symptom đã xác nhận:

Icon View không xuất hiện trong cột Action trên Interview Schedule đối với Role C và Role D, trong khi REF 14 yêu cầu icon này available cho tất cả role.

Test case bị ảnh hưởng: TC16-16 – P1.

Layer nghi ngờ: FE/UI rendering hoặc permission integration, nhưng chưa đủ evidence để xác định layer gây lỗi.

Root cause: CHƯA VERIFY.

Hiện chưa thể kết luận đây là:

FE không render icon,
FE mapping permission sai,
BE trả permission sai,
hay một cơ chế authorization khác.

Evidence quan trọng nhất cần bổ sung: response API + DOM/DevTools + kiểm tra Role A/B.
