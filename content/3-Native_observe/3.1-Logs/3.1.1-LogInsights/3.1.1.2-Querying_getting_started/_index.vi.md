---
title : "Bắt đầu với truy vấn"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.1.1.2 </b> "
---

Chúng ta sẽ bắt đầu với các truy vấn đơn giản:
 - Truy vấn mặc định
 - Tìm kiếm tin nhắn chứa text (sử dụng *filter*)
 - Đếm sự kiện (sử dụng *stats*) và xem kết quả dưới biểu đồ thời gian. Ta sẽ chạy truy vấn cho mỗi thể loại, giải thích xem nó làm gì, và xem kết quả ra sao.

### Truy vấn mặc định

1. Trên Bảng điều khiển quản lý AWS, trong menu *Services*, nhấp vào **CloudWatch**.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Logs Insights**.

3. Từ **Select log group(s)**, chọn */ecs/PetListAdoptions*.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.2/001.png)

Bạn sẽ thấy một truy vấn mẫu được tự động đặt trong trường truy vấn.

Truy vấn này sẽ:

 - Hiển thị dấu thời gian và tin nhắn bằng cách sử dụng lệnh *fields*,

 - Sắp xếp theo thời gian dấu thời gian theo thứ tự giảm dần (desc), và sau đó

 - Giới hạn hiển thị chỉ với 20 kết quả cuối cùng.

Điều này luôn là một điểm khởi đầu tốt để xem các sự kiện log trông như thế nào trong log group của bạn.

{{% notice note %}}
**Các trường bắt đầu bằng @**\
Các trường bắt đầu bằng "@" là các trường mà CloudWatch tự động tạo ra.\
Trường @message chứa raw log chưa được phân tích.
{{% /notice %}}

4. Nhấp vào nút **Run query** và xem kết quả.
![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.2/002.png)

Ở đầu bạn có thể thấy một biểu đồ cột hiển thị phân phối các sự kiện log theo thời gian mà chúng khớp với truy vấn của bạn.

Dưới đây, bạn có thể thấy các sự kiện khớp với truy vấn của bạn. Bạn có thể nhấp vào mũi tên bên trái của mỗi dòng để mở rộng sự kiện. Trong trường hợp này, vì sự kiện là dạng JSON, chúng ta thấy nó hiển thị dưới dạng một danh sách các tên trường, với giá trị tương ứng bên cạnh. Ví dụ, trong sự kiện được mở rộng, tôi thấy tôi có một trường có tên là *petcolor*, và nó có giá trị là *white*.

### Tìm các tin nhắn chứa văn bản

Chúng ta có thể sử dụng lệnh *filter* để chỉ định các sự kiện log chúng ta muốn sử dụng.

Lệnh *filter* có thể được sử dụng để lấy một trường khớp với mong muốn, sự kiện chứa văn bản được chỉ định, thêm điều kiện cho các trường dạng số (numeric fields), kiểm tra xem một trường có tồn tại không và nhiều hơn nữa.

{{% notice note %}}
**Về cú pháp truy vấn** \
Đối với thông tin chi tiết hơn, hãy xem tài liệu AWS về [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html).
{{% /notice %}}

5. Xóa truy vấn đã có trong trình soạn truy vấn và sao chép và dán vào truy vấn sau đây:

```sql
fields @timestamp, @message
| filter @message like /brown/
| sort @timestamp desc
| limit 20
```

Truy vấn này áp dụng một filter trên các tin nhắn và chỉ lấy các bản ghi chứa chuỗi brown trong log (trong trường *@message*).

Nó cũng sắp xếp các sự kiện được tìm thấy theo thứ tự thời gian giảm dần và chỉ trả về 20 sự kiện này.

6. Nhấp vào nút **Run query**.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.2/003.png)

### Đếm sự kiện (events) và xem kết quả dưới biểu đồ thời gian (time chart).

Chúng ta có thể sử dụng lệnh *stats* để tổng hợp dữ liệu.

Lệnh *stats* có thể được sử dụng để đếm số lượng sự kiện, hoặc tính toán thống kê trên một trường cụ thể, như trung bình, tổng, max, min và nhiều hơn nữa.

{{% notice note %}}
**Về cú pháp truy vấn** \
Đối với thông tin chi tiết hơn, hãy xem tài liệu AWS về [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html).
{{% /notice %}}

7. Xóa truy vấn đã có trong trình soạn truy vấn và sao chép và dán vào truy vấn sau đây:

```sql
fields @timestamp, @message
| stats count(@message) as number_of_events by bin(5m)
| limit 20
```

Truy vấn này trả về kết quả là số lượng tin nhắn được ghi lại trong các khoảng 5 phút.

8. Bạn cũng có thể hiển thị kết quả dưới dạng biểu đồ bằng cách nhấp vào tab **Visualization** trong khu vực kết quả như được hiển thị dưới đây.

Chú ý rằng bạn cũng có thể thêm biểu đồ vào CloudWatch Dashboard, xuất ra csv và nhiều hơn nữa.

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.2/004.png)