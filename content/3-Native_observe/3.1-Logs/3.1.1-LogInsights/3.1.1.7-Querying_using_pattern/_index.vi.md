---
title : "Truy vấn sử dụng pattern"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b> 3.1.1.7 </b> "
---

Trong phần này, chúng ta sẽ khám phá cú pháp *pattern*. Bạn có thể truy vấn bằng cú pháp *pattern* để nhóm hoặc tổng hợp log thành các patterns phổ biến.

### Sử dụng cú pháp pattern hữu ích như nào?

Tìm kiếm và lọc qua các lượng lớn log có thể khó khăn đặc biệt khi bạn đang trong tình huống cần phải nhanh chóng điều tra lỗi. Truy vấn bằng cú pháp *pattern* có thể giúp bạn xác định các dòng log có chi phí cao, giám sát các lỗi xảy ra, tìm các pattern tái diễn trong log của bạn và cung cấp số lượng các pattern được sắp xếp theo mức độ nghiêm trọng.

### Kết quả pattern trả về trông như thế nào?

Hãy bắt đầu bằng một truy vấn *pattern* đơn giản.

1. Thay đổi log group thành */ecs/PetSearch*. Từ menu drop down **Select log group(s)**, chọn */ecs/PetSearch*.

2. Sao chép truy vấn dưới đây và **Run query**.
```sql
pattern @message
```
Bạn sẽ thấy kết quả, tuy nhiên chúng hiện đang được mặc định sắp xếp theo trường *ratio*, hiển thị tỷ lệ sự kiện log trong khoảng thời gian được chọn và các nhóm log phù hợp với pattern được xác định. Mục tiêu của chúng ta là sắp xếp các log theo mức độ nghiêm trọng(**severity level**). Hãy kiểm tra một số trường hợp sử dụng trước khi sắp xếp kết quả theo mức độ nghiêm trọng.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.7/001.png)

### Sử dụng **pattern** với **filter**, **parse** và **sort**

Để điều chỉnh truy vấn, bạn có thể kết hợp *pattern* với các lệnh *filter*, *parse* hoặc *sort*.

Đầu tiên sử dụng truy vấn *filter* độc lập trong cùng log group */ecs/PetSearch*.

```sql
filter @message like /Error Code: AccessDenied/
```

Kết quả sẽ hiển thị tất cả các log bị từ chối truy cập trong các dòng riêng lẻ. Tuy nhiên, bạn muốn xem các log trong một định dạng nhóm phù hợp với pattern */Error Code: AccessDenied/*.

Chạy truy vấn dưới đây và kiểm tra kết quả.

```sql
filter @message like /Error Code: AccessDenied/
| pattern @message
```

Bạn sẽ thấy rằng các kết quả đã được tổng hợp lại với nhau chứa */Error Code: AccessDenied/* với số lượng.

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.7/002.png)

Bây giờ, hãy chạy lệnh dưới đây với sự kết hợp của *filter*, *parse* và *sort* với *pattern* để có kết quả cụ thể hơn.

```sql
filter @message like /ERROR/
| parse @message 'Error Code: *' as errcode
| pattern errcode
```

Dưới đây là kết quả.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.7/003.png)

### Sắp xếp theo Mức độ nghiêm trọng

Bây giờ, sau khi đã khám phá một số truy vấn *pattern*, hãy chạy truy vấn dưới đây để sắp xếp theo *severityLabel*.

```sql
pattern @message
| sort @severityLabel
| limit 20
```

Điều này sẽ sắp xếp các nhật ký theo mức độ nghiêm trọng (ERROR là ưu tiên cao nhất) và cung cấp cho bạn số lượng của số sự kiện log phù hợp với pattern được xác định.

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.7/004.png)

{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-Pattern.html" %}}Đọc thêm về chủ đề này...{{% /button %}}

