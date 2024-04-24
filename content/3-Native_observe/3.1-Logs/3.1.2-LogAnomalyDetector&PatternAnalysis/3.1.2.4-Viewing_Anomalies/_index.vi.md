---
title : "Xem các Sự bất thường"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 3.1.2.4 </b> "
---

Sau khi tạo các log anomaly detectors, bạn có thể sử dụng bảng điều khiển CloudWatch để xem các bất thường (anomalies) đã tìm thấy.

1. Trên thanh điều hướng bên trái của CloudWatch, nhấp vào **Log Anomalies**.

Trong bảng điều khiển được hiển thị dưới đây, bạn sẽ thấy tất cả các bất thường được hiển thị với mức ưu tiên(priority level), log trend và tần suất xuất hiện của mẫu. Lưu ý rằng có một sự tăng về giao dịch với puppies trên các trang web của chúng tôi. Ngoài ra, có hai bất thường được gắn nhãn với mức ưu tiên medium chứa các lỗi cần phải được điều tra kỹ hơn.

![001](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.4/001.png)

Để xem thông tin chi tiết hơn, bạn có thể nhấp vào *Anomaly* và nó sẽ hiển thị một bảng điều khiển hiển thị pattern, các log sample được ghi lại và biểu đồ hiển thị số lần xảy ra sự kiện trong một khoảng thời gian.

![002](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.4/002.png)

Từ đây, bạn có thể tạo một CloudWatch Alarm để thông báo cho team hoặc phản ứng tự động về một bất thường cụ thể đã được phát hiện trong log.

Ngoài ra, bạn có thể tùy chọn để khiến một bất thường tạm thời hoặc vĩnh viễn. Hành động này làm cho anomaly detector ngừng đánh dấu sự xuất hiện trong một khoảng thời gian bạn chỉ định.