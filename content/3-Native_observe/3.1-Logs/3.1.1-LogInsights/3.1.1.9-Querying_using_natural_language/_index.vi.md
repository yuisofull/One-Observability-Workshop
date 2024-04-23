---
title : "Truy vấn sử dụng ngôn ngữ tự nhiên"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b> 3.1.1.9 </b> "
---

{{% notice note %}}
Tính năng này đang trong phiên bản xem trước ở US East (N. Virginia) và US West (Oregon) cho CloudWatch Logs và có thể thay đổi.
{{% /notice %}}

CloudWatch Logs hỗ trợ khả năng truy vấn bằng ngôn ngữ tự nhiên để giúp bạn tạo và cập nhật các truy vấn cho CloudWatch Logs Insights và CloudWatch Metrics Insights.

Với tính năng này, bạn có thể đặt câu hỏi hoặc mô tả dữ liệu CloudWatch Logs bạn đang tìm kiếm bằng tiếng Anh thông thường. Khả năng ngôn ngữ tự nhiên tạo ra một truy vấn dựa trên prompt mà bạn nhập vào và cung cấp một giải thích từng dòng về cách hoạt động của truy vấn. Bạn cũng có thể cập nhật truy vấn của mình để đi sâu hơn về dữ liệu của bạn.

Tùy thuộc vào môi trường của bạn, bạn có thể nhập lời nhắc như  "What are the top 100 source IP addresses by bytes transferred?" ("Các địa chỉ IP trong top 100 theo số byte được truyền?") và  "Find the 10 slowest Lambda function requests."("Tìm 10 request Lambda function chậm nhất.")

{{% button href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatchLogs-Insights-Query-Assist.html" %}}Đọc thêm về chủ đề này...{{% /button %}}

### Tạo truy vấn

{{% notice info %}}
**Các yếu tố cần xem xét** \
Các truy vấn được tạo ra bởi trí tuệ nhân tạo sinh ra và phụ thuộc vào các yếu tố bao gồm dữ liệu được chọn và có sẵn trong tài khoản của bạn. Vì những lý do này, kết quả của bạn có thể thay đổi.
{{% /notice %}}

Hãy bắt đầu với việc tạo một truy vấn đơn giản. Để tạo một truy vấn CloudWatch Logs Insights với tính năng này, hãy chọn một log group bạn muốn truy vấn, trong trường hợp này chúng tôi sẽ chọn */ecs/PetListAdoptions* và sau đó nhấp vào **Query generator** ở dưới cùng như được đánh dấu bằng mũi tên đỏ.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/001.png)

Trong trường văn bản mới này, chúng ta có thể viết prompt của mình bằng ngôn ngữ tự nhiên. Hãy bắt đầu với một prompt đơn giản:

```sql
show me the last 50 log entries in descendent order
```

Nhấp vào **Generate new query**. Như bạn có thể thấy, một truy vấn mới đã tự động được tạo thêm lấy 50 log cuối cùng.

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/002.png)

Cuối cùng nhấp vào **Run query**. Nhớ rằng **Generate new query** không tự động chạy truy vấn(**Run query**) mới được tạo.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/003.png)

Bây giờ, hãy thêm một số *filter* như chúng ta đã thấy trong các phần trước. Giả sử rằng chúng ta muốn kiểm tra tất cả các log cho các loài vật có màu nâu. Sao chép văn bản sau đây và nhấp vào **Update query** để giữ cấu trúc truy vấn cơ bản giống như đã tạo và chỉ thêm các phần thay đổi, đó là filter chuỗi màu nâu(brown).

```sql
show me the last 50 log entries containing the string brown
```

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/004.png)

Prompt này thêm vào truy vấn một filter cho trường @message chứa chuỗi nâu(brown).

Nếu logs của chúng ta được tạo ra dưới dạng JSON, Log Insights sẽ có khả năng phân tích cú pháp mỗi thuộc tính để truy vấn chúng một cách độc lập. Hãy cụ thể hơn, thay vì tìm kiếm một chuỗi trong sự kiện log hoàn chỉnh, hãy tách với thuộc tính `petcolor` được tìm thấy trong log.

```sql
show me the last 50 log entries where the petcolor is brown in descendent order
```

![005](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/005.png)

Nếu bạn không nhận được cùng một đầu ra, hãy thử nói rõ hơn, ví dụ: `how me the last 50 log entries where the custom field petcolor is brown in descendent order`và thử lại.

### Thêm giải thích truy vấn

Khi sử dụng trình tạo truy vấn, bạn sẽ có thể nhận được giải thích về truy vấn để giúp bạn học ngôn ngữ bao gồm các tính năng nâng cao hơn. Để kích hoạt tính năng này, hãy nhấp vào **Preferences** ở bên phải và bật cả hai tính năng: **Include your prompt as comment when generating query using query generator** và **Include explanation of the generated query as comments when using query generatorn**.

Sau khi bật, hãy thử một số lệnh *stats*. Như chúng ta đã thấy trong các phần trước, chúng ta có thể muốn phân tích bao nhiêu con vật được nhận nuôi(adopted) nhóm theo loại vật nuôi, như *puppy* hoặc *bunny*:

```sql
show me the logs for the count of pet type fields if the value is not empty
```

![006](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/006.png)

Ngoài ra, nếu bạn không nhận được truy vấn mong đợi, ví dụ, nếu không có xác nhận không được trống được thêm vào, bạn có thể thử viết lại và cập nhật truy vấn cho đến khi bạn đạt được truy vấn mong đợi của mình.

Cuối cùng, chúng ta có thể thử các loại logs khác, hãy chuyển sang nhóm nhật ký */ecs/PetSearch* chứa các logs cho dịch vụ Java.

Nếu chúng tôi đang troubleshoot các lỗi cho dịch vụ cụ thể này, chúng ta có thể thử prompt sau:

```sql
give me all ERROR logs level and show the exception field
```

![007](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.9/007.png)