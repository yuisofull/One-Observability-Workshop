---
title : "Log Insights"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1.1 </b> "
---

CloudWatch Logs Insights cho phép bạn tương tác tìm kiếm và phân tích dữ liệu log của mình trong Amazon CloudWatch Logs. Bạn có thể thực hiện các truy vấn để giúp bạn phản ứng nhanh chóng một cách hiệu quả hơn với các vấn đề trong lúc vận hành. Nếu một vấn đề xảy ra, bạn có thể sử dụng CloudWatch Logs Insights để xác định nguyên nhân tiềm ẩn cũng như các thay đổi đã thực hiện.

CloudWatch Logs Insights có một ngôn ngữ truy vấn được tạo ra với một số lệnh đơn giản nhưng mạnh mẽ. CloudWatch Logs Insights cung cấp các truy vấn mẫu, mô tả lệnh, tự động hoàn thành truy vấn và khám phá log để giúp bạn bắt đầu. Các truy vấn mẫu đã được bao gồm cho một số loại logs dịch vụ AWS.

CloudWatch Logs Insights tự động khám phá các vùng (fields) trong các log từ các dịch vụ AWS như Amazon Route 53, AWS Lambda, AWS CloudTrail và Amazon VPC, cũng như bất kỳ ứng dụng hoặc lod tùy chỉnh nào phát sinh sự kiện log dưới dạng JSON.

Bạn có thể sử dụng CloudWatch Logs Insights để tìm kiếm log đã được gửi đến CloudWatch Logs vào ngày 5 tháng 11 năm 2018 hoặc sau đó.

Bạn có thể lưu trữ các truy vấn bạn đã tạo. Điều này có thể giúp bạn chạy các truy vấn phức tạp khi cần thiết, mà không cần phải tạo lại chúng mỗi lần bạn muốn chạy chúng.

{{% notice note %}}
**Chúng ta sẽ sử dụng dữ liệu nào?**

Chúng ta sẽ sử dụng log được tạo ra bởi ứng dụng PetSite trong module này. Bạn có thể sử dụng log của riêng bạn, nhưng định dạng và logic truy vấn sẽ khác nhau.
{{% /notice %}}


{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html" %}}Đọc thêm về chủ đề này {{% /button %}}