---
title : "Phát Hiện Log Bất Thường & Phân Tích Pattern"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.1.2 </b> "
---

Phân tích một lượng dữ liệu lớn theo ngày cho việc phân tích log trở nên khó khăn hơn khi xác định các thay đổi trong log events theo thời gian và các vấn đề không lường trước.

Amazon CloudWatch Logs Anomaly detection cho phép bạn tóm tắt nhanh chóng các mục log thành một số pattern bằng cách sử dụng machine learning. Bạn có thể sử dụng CloudWatch Logs Anomaly detection để:

- Luôn luôn có phát hiện bất thường
- So sánh các thay đổi trong log để xác định nguyên nhân gốc rễ của vấn đề
- Tóm tắt hàng ngàn log một cách nhanh chóng

CloudWatch Log Anomaly detection là một tính năng được quản lý hoàn toàn và hoạt động ngay lập tức cho tất cả các loại log mà không cần kiến thức về machine learning để sử dụng.

{{% notice note %}}
**Chúng ta sẽ sử dụng dữ liệu nào?**
\
Chúng ta sẽ sử dụng log được tạo ra bởi ứng dụng PetSite trong module này. Bạn có thể sử dụng log của riêng bạn, nhưng định dạng và logic truy vấn sẽ khác nhau.
{{% /notice %}}


{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/LogsAnomalyDetection.html" %}}Đọc thêm về chủ đề này... {{% /button %}}