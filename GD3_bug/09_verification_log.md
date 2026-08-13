# Dự đoán kết quả Verify — UC16 View Interview Schedule

> Đây là dự đoán trước khi test, dùng để so sánh với kết quả thực nghiệm thật (con người chạy 100%, AI không thể verify).

## Bug 1 — Thiếu cột Interviewer/Result

| Hypothesis | Số lần chạy dự kiến | Dự đoán kết quả (X/20) | Dự đoán kết luận |
|---|---|---|---|
| H1 — FE thiếu config 2 cột | 20 | 19/20 | Confirmed |
| H2 — FE mapping Result vào Status | 20 | 3/20 | Rejected |
| H3 — API không trả interviewer/result | 20 | 1/20 | Rejected |

## Bug 2 — Sai message no-result

| Hypothesis | Số lần chạy dự kiến | Dự đoán kết quả (X/20) | Dự đoán kết luận |
|---|---|---|---|
| H1 — FE hard-code sai message | 20 | 20/20 | Confirmed |
| H2 — FE dùng sai message key/constant | 20 | 17/20 | Confirmed (phụ) |
| H5 — BE trả sai message | 10 | 1/10 | Rejected |

## Bug 3 — Thiếu filter Interviewer

| Hypothesis | Số lần chạy dự kiến | Dự đoán kết quả (X/30) | Dự đoán kết luận |
|---|---|---|---|
| H1 — FE chưa implement component | 30 | 30/30 | Confirmed |
| H2 — Điều kiện render sai | 20 | 2/20 | Rejected |
| H5 — Feature flag disable | 20 (10 ON/10 OFF) | 0/10 ON | Rejected |

## Bug 4 — Status "Open" thay vì "New"

| Hypothesis | Số lần chạy dự kiến | Dự đoán kết quả | Dự đoán kết luận |
|---|---|---|---|
| H1 — FE hard-code "Open" | 1 (code review) | Tìm thấy hard-code | Confirmed |
| H2 — FE mapping enum sai | 10 | 2/10 | Rejected |
| H3 — BE trả sai enum "Open" | 10 | 1/10 | Rejected |

## Bug 5 — Thiếu action View cho Role C/D

| Hypothesis | Số lần chạy dự kiến | Dự đoán kết quả (X/20/role) | Dự đoán kết luận |
|---|---|---|---|
| H1 — FE sai logic permission render | 20 | 19/20 | Confirmed |
| H2 — Config action list thiếu View | 20 | 18/20 | Confirmed (phụ) |
| H4 — BE không trả capability View | 10 | 6/10 | Chưa rõ ràng |
