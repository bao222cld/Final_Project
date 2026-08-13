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
