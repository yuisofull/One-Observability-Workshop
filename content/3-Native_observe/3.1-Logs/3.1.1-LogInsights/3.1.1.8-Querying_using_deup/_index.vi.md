---
title : "Truy vấn sử dụng deup"
date :  "`r Sys.Date()`" 
weight : 8 
chapter : false
pre : " <b> 3.1.1.8 </b> "
---

Trong phần này, chúng ta sẽ khám phá cú pháp *dedup*. Cú pháp *dedup* cho phép bạn loại bỏ các kết quả trùng lặp từ log của bạn dựa trên các giá trị trường cụ thể.

### Truy vấn với và không có dedup

Hãy chạy một truy vấn đơn giản với và không có *dedup* và kiểm tra kết quả.

Thay đổi log group thành */ecs/PetSearch*. Từ menu drop down **Select log group(s)**, chọn */ecs/PetSearch*.

Chạy truy vấn KHÔNG DÙNG *dedup* và kiểm tra kết quả.

```sql
fields @timestamp, message 
| sort @timestamp asc
```

Kiểm tra số lượng kết quả có cùng message mà không sử dụng *limit*. Mặc định, sẽ hiển thị 1000 hàng chứa cả chi tiết log.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.8/001.png)

Bây giờ, hãy chạy cùng một truy vấn CÓ *dedup*.

```sql
fields @timestamp, message 
| sort @timestamp asc
| dedup message
```
Bạn sẽ thấy rằng *dedup* loại bỏ các bản sao và chỉ liệt kê các message một cách độc nhất.

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.8/002.png)

{{% notice info %}}
Các giá trị trùng lặp sẽ bị loại bỏ dựa trên thứ tự sắp xếp, chỉ giữ lại kết quả đầu tiên trong thứ tự sắp xếp. Vì thế khuyến khích sắp xếp kết quả trước khi sử dụng lệnh *dedup*.
{{% /notice %}}

### Sử dụng *dedup* với *filter*

Bây giờ chúng ta đã biết sự khác biệt giữa việc chạy các truy vấn CÓ và KHÔNG có *dedup*, hãy thử một số ví dụ khác với cú pháp *filter*.

Hãy thay đổi log group thành */ecs/PayForAdoption* từ menu drop down **Select log group(s)**.

Trong truy vấn dưới đây, chúng tôi muốn lấy PetType và PetId, tuy nhiên, hãy chạy truy vấn trước mà không có *dedup*.

```sql
fields @timestamp, @message, @log
| filter PetType = "kitten" and PetId = "022"
| sort @timestamp desc
```

Kiểm tra kết quả đã chọn lọc dưới đây cho *PetType* và *PetId*. Cũng lưu ý các ID nhật ký dưới trường *@log*.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.8/003.png)

Bây giờ hãy thêm lệnh *dedup* để loại bỏ các kết quả trùng lặp từ trường *@log*.

```sql
fields @timestamp, @message, @log
| filter PetType = "kitten" and PetId = "022"
| sort @timestamp desc
| dedup @log
```

Bây giờ điều đang xảy ra ở đây là khi bạn chỉ định một trường với *dedup*, chỉ một sự kiện log được trả về cho mỗi giá trị duy nhất của trường đó, với sự kiện mới nhất của nó. Trong trường hợp của chúng tôi ở đây, đó là giá trị duy nhất mới nhất của trường *@log* với một ID nhật ký duy nhất.

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.8/004.png)

Nếu bạn chỉ định nhiều trường, sau đó một log event được trả về cho mỗi kết hợp giá trị duy nhất của các trường đó, trong trường hợp của chúng tôi, nó sẽ trả về các kết hợp duy nhất của các giá trị từ cả hai trường *@message* và *@log*.

Chạy truy vấn dưới đây và kiểm tra kết quả.

```sql
fields @timestamp, @message, @log
| filter PetType = "kitten" and PetId = "022"
| sort @timestamp desc
| dedup @log, @message
```

Mở rộng một sốlog và bạn sẽ thấy rằng mặc dù các giá trị *PetType* và *PetId* được chọn lọc, kết quả *FullName* của khách hàng là duy nhất trong trường *@message*.

Để kiểm tra thêm các trường để sử dụng với *dedup*, nhấp vào **Discovered fields** ở bên phải của bảng điều khiển CloudWatch Log Insights.

{{% notice info %}}
**dedup cho các lời gọi hàm chậm** \
dedup cũng giúp tìm kiếm các lời gọi hàm chậm và loại bỏ các request trùng lặp có thể phát sinh từ việc request liên tục hoặc client-side code, ví dụ: `dedup @requestID`.
{{% /notice %}}

{{% notice info %}}
**dedup cho troubleshoot network** \
dedup rất hữu ích khi bạn muốn kiểm tra các log events từ trường(field) *server* duy nhất với một mức độ nghiêm trọng(*severity* type). Khi sử dụng với cú pháp *filter*, nó cũng giúp gỡ lỗi vấn đề kết nối mạng bằng cách tìm các giá trị duy nhất từ nguồn hay địa chỉ IP ở trong *vpc-flow-logs*.
{{% /notice %}}

{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-Dedup.html" %}}Đọc thêm về chủ đề này...{{% /button %}}