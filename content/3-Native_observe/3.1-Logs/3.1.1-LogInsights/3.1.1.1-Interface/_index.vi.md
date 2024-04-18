---
title : "Navigating the Interface"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1.1.1 </b> "
---

Trước khi chúng ta tiếp xúc với ngôn ngữ truy vấn, ta sẽ cần biết về giao diện Logs Insights console và các tính năng chúng ta có thể sử dụng, như khám phá trường (fields) tự động, truy vấn mẫu, truy vấn đã lưu và lịch sử truy vấn. 

### Khám phá trường tự động

CloudWatch có thể tự động khám phá các trường từ dữ log của bạn.

1. Trong Bảng điều khiển quản lý AWS, trên menu Services, nhấp vào CloudWatch.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Logs Insights**.

3. Chọn *log group* có tên **/ecs/PetListAdoptions** từ **Select log group(s)**.

Bạn sẽ thấy một truy vấn mẫu được tự động đặt trong vùng truy vấn.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/001.png)

4. Nhấp **Run query**
{{% notice note %}}
**Chúng ta sẽ sử dụng dữ liệu nào?**

Chúng ta sẽ sử dụng log được tạo ra bởi ứng dụng PetSite trong module này. Bạn có thể sử dụng log của riêng bạn, nhưng định dạng và logic truy vấn sẽ khác nhau.
{{% /notice %}}


{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html" %}}Đọc thêm về chủ đề này {{% /button %}}