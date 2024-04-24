---
title : "So Sánh các Patterns Khác Nhau với Khoảng Thời Gian Trước Đó"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.1.2.2 </b> "
---

Bạn có thể sử dụng CloudWatch Logs Insights để **So sánh** các thay đổi trong các log events của bạn qua thời gian. Bạn có thể so sánh các log event trong một khoảng thời gian gần đây với các log từ khoảng thời gian ngay trước đó hoặc các khoảng thời gian tương tự trong quá khứ. Điều này có thể giúp bạn xác định xem một lỗi trong log của bạn mới xuất hiện gần đây hay đã xảy ra trước đó, và có thể giúp bạn tìm ra các trends khác.

1. Bằng bộ chọn phạm vi thời gian, chọn **Compare**.

![001](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.2/001.png)

2. Sau đó, chọn **previous time** (khoảng thời gian trước đó) mà bạn muốn so sánh với các log gốc, và chọn **Apply**.

3. Chọn **Run query**.

{{% notice info %}}
Thông tin cần biết: Nút **Compare** trong phạm vi thời gian trên trang Logs Insights cho phép bạn so sánh nhanh chóng kết quả truy vấn cho phạm vi thời gian được chọn với một giai đoạn trước đó, chẳng hạn như ngày trước đó, tuần trước hoặc tháng trước. Điều này giúp tiết kiệm thời gian để xem những thay đổi đã xảy ra so với một bản ổn định trước đó.
{{% /notice %}}

![002](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.2/002.png)

Chúng ta có thể suy ra rằng một số pattern đã giảm, tăng và không còn xuất hiện nữa, khi chúng ta so sánh với giai đoạn trước đó.

Chúng ta cũng có thể kiểm tra kỹ hơn lỗi **ERROR** của chúng ta bằng cách chọn tùy chọn Inspect trên một trong các patterns trong danh sách và xem các log samples, các giá trị token và các patterns liên quan.