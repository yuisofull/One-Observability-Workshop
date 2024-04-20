---
title : "Truy vấn - sử dụng trường(fields)"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 3.1.1.4 </b> "
---

Như chúng ta đã đề cập, Logs Insights tự động trích xuất các trường từ các sự kiện có định dạng JSON (bạn có thể đọc thêm tại [Fields in JSON logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_AnalyzeLogData-discoverable-fields.html)). Tuy nhiên, có khả năng một số log của bạn sẽ không có định dạng JSON, hoặc việc tách các trường sẽ không có tác dụng với bạn. Dù sao bạn cũng sẽ muốn phân chia một trường thành các phần nhỏ hơn.

### Tại sao việc phân chia dữ liệu log thành các trường khác nhau hữu ích?

Việc phân chia dữ liệu của chúng ta thành các trường cho phép chúng ta lựa chọn những gì muốn hiển thị, cũng như tổng hợp hoặc áp dụng logic vào dữ liệu/các trường chúng ta quan tâm.

Trong ví dụ này, chúng ta sẽ chạy một truy vấn để sử dụng các log từ ứng dụng petsite và tìm kiếm các sự kiện nhận nuôi (adoption events). Chúng ta muốn xem mẫu nhận nuôi của các loại thú cưng khác nhau qua thời gian.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/001.png)

Chúng ta sẽ xây dựng truy vấn từng bước một:

 - Bước 1: Lọc các sự kiện mà chúng ta quan tâm
 - Bước 2: Tạo các trường - phân chia dữ liệu thành các trường hữu ích
 - Bước 3: Phân tách trường mà chúng ta quan tâm một lần nữa
- Bước 4: Tích hợp dữ liệu và chọn cách hiển thị nó

### Sự kiện(events) sẽ trông như thế nào?

Trước khi chúng ta bắt đầu phân tích cách phân chia dữ liệu, chúng ta cần xem xét định dạng của các sự kiện mà chúng ta quan tâm.

1. Trong Bảng điều khiển quản lý AWS trên menu Dịch vụ, nhấp vào **CloudWatch**.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Logs Insights**.

3. Từ menu thả xuống **Select log group(s)**, chọn nhóm log `/ecs/PayForAdoption`.

4. Để truy vấn mặc định và **Run query**. Bạn nên thấy các sự kiện tương tự như dưới đây.

> POST /api/home/completeadoption?petId=024&petType=bunny HTTP/1.1 11.0.13.138:1876 200

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/002.png)

Chúng ta có 5 trường, được phân tách bằng dấu cách. Chúng ta quan tâm đến trường thứ 2, vì nó chứa petType.

### Bước 1: Lọc các sự kiện quan tâm

Chúng ta sẽ bắt đầu bằng cách thu hẹp các sự kiện mà truy vấn sẽ trả về. Dây bước đầu tiên quan trọng trong truy vấn - nó đảm bảo chúng ta chỉ nhìn vào dữ liệu quan trọng trong kết quả.

5. Sửa truy vấn của bạn để phù hợp với những gì được hiển thị dưới đây. Ta sử dụng lệnh filter để giữ lại các sự kiện chứa chuỗi POST.

```sql
filter @message like /POST/ 
| fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

Càng cụ thể ở đây, việc xác định chính xác sẽ dễ dàng hơn để biết chúng ta đang sử dụng đÚng dữ liệu. Chúng ta có thể thêm vào bộ lọc để chỉ nhận các sự kiện completeadoption. Ví dụ:

```sql
filter @message like /POST/ and @message like /completeadoption/
| fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

6. Chạy truy vấn và kiểm tra bạn chỉ nhìn thấy các sự kiện POST, completeadoption.

### Bước 2: Tạo các trường - phân chia dữ liệu thành các trường hữu ích
Bây giờ chúng ta đã biết định dạng và chỉ nhìn vào các sự kiện chúng ta quan tâm, chúng ta sẽ phân chia dữ liệu của mình thành các trường bằng cách sử dụng lệnh `parse`.

Chúng ta làm điều này bằng cách sử dụng lệnh `parse`. Chúng ta có thể sử dụng lệnh parse với một mẫu đơn giản (được biết đến là biểu thức glob), hoặc sử dụng regex (cú pháp biểu thức chính quy). Đối với ví dụ này, một mẫu đơn giản sẽ đủ.

Chúng ta có các trường được phân tách bằng dấu cách, vì vậy chúng ta đặt một * (wildcard) vào, sau đó chỉ định tên chúng ta muốn cho mỗi trường. Chúng ta cũng đã phân tách ra địa chỉ IP và cổng trong trường thứ 4 bằng cách sử dụng dấu : (ip:port).

Hãy xem lệnh parse dưới đây.

```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| limit 20
```

7. Cập nhật truy vấn của bạn để phù hợp với điều được hiển thị dưới đây, và **Run query**. Bây giờ bạn nên thấy các cột cho mỗi trường chúng ta đã chỉ định, và dữ liệu được hiển thị cho mỗi sự kiện.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/003.png)

{{% notice info %}}
**Đọc thêm về parse**\
Bạn có thể xem [cú pháp truy vấn của Log Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html) để biết thêm thông tin về lệnh `parse`, cũng như một số ví dụ về việc sử dụng cả biểu thức glob và regex.
{{% /notice %}}


### Bước 3: Phân tích chi tiết trường sâu hơn

Chúng ta đã tìm gần đến dữ liệu chúng ta muốn rồi, nhưng *petType* vẫn chỉ là một phần của trường *request*. Sẽ tốt hơn nếu chúng ta phân chia nó thành các phần nhỏ hơn và có một trường chỉ dành cho *petType*.

Chúng ta đã có thể thực hiện điều này trong lệnh `parse` ban đầu của mình, nhưng đôi khi có vài lý do để làm việc này riêng biệt.

Hãy xem lệnh parse mà chúng ta đã sử dụng ở Bước 2.

```sql
parse @message "* * * *:* *" as method, request, protocol, ip, port, status
```

Ở đây, chúng ta đã phân tách trường được gọi là *@message* xuống một cấp. Bạn có thể thấy rằng chúng ta đã chỉ định trường *@message* ngay sau lệnh *parse*. Chúng ta có thể chỉ định bất kỳ trường nào ở đây.

Trong ví dụ này, chúng ta muốn phân tích cú pháp trường yêu cầu để trích xuất *petType*. Vì vậy, chúng ta chỉ định trường yêu cầu trực tiếp sau lệnh *parse* và bao gồm glob pattern và tên trường mới phù hợp.

```sql
parse request "*?petId=*&petType=*" as requestURL, id, type
```

Chúng ta vẫn đang sử dụng glob pattern, nhưng ở đây chúng ta đã cụ thể hóa hơn để các trường chứa chính xác dữ liệu chúng ta quan tâm. Bạn có thể thấy ba dấu * (wildcards) và 3 tên trường.

8. Cập nhật truy vấn của bạn để phù hợp với truy vấn đầy đủ được hiển thị dưới đây, và **Run query**. Bạn nên thấy các cột mới cho *requestURL*, *id*, và *type*.

```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| limit 20
```
![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/004.png)

{{% notice info %}}
**Chỉ hiển thị một số trường**\
Nếu bạn cảm thấy kết quả của mình đang trở nên phức tạp, và bạn chỉ muốn xem một số trường, thì bạn có thể sử dụng lệnh `display` để chỉ định tên trường và thứ tự. Tất cả các trường khác sẽ bị ẩn đi:
```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| display @timestamp, type
| limit 20
```

{{% /notice %}}

### Bước 4: Tổng hợp dữ liệu

Chúng ta đã trích xuất dữ liệu của mình với mục đích xem xét pattern tự nuôi nhận nuôi (adoptions) của các loại vật nuôi(pet) khác nhau theo thời gian. Bây giờ chúng ta có trường loại vật nuôi (pet type), chúng ta đã sẵn sàng để tổng hợp.

Ở đây, chúng ta sử dụng lệnh *stats*. Lệnh *stats* cho phép chúng ta tổng hợp dữ liệu của mình qua các nhóm khác nhau. Chúng ta sẽ xem qua một số ví dụ.

9. Một tổng hợp đơn giản sẽ là đếm có bao nhiêu sự kiện cho mỗi loại vật nuôi, nói cách khác, đếm và nhóm theo loại.
Sao chép truy vấn dưới đây và **Run query**.

```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| stats count() by type
```

10. Chúng ta có thể nhóm theo nhiều trường, bao gồm một trường thời gian. Ví dụ, có bao nhiêu sự kiện cho mỗi loại vật nuôi trong mỗi khoảng thời gian 5 phút?
Sao chép truy vấn dưới đây và **Run query**.
```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| stats count() by type, bin(5m)
```

![005](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/005.png)

Kết quả cho một bảng dữ liệu được nhóm theo cả loại vật nuôi và các khung thời gian.

11. Để hiển thị biểu đồ thời gian với một đường cho mỗi loại vật nuôi, chúng ta cần sử dụng lệnh stats một cách khác.
Sao chép truy vấn dưới đây và **Run query**.
```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| stats sum(type="puppy") as puppy, sum(type="kitten") as kitten, sum(type="bunny") as bunny by bin(5m)
```
![006](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/006.png)

Kết quả có định dạng khác với truy vấn trước đó. Lần này chúng ta có một cột cho mỗi loại vật nuôi. Định dạng này sẽ cho phép chúng ta xem biểu đồ thời gian của dữ liệu trên mỗi loại vật nuôi.

12. Nhấp vào tab **Visualization** để xem biểu đồ thời gian của bạn. Đảm bảo rằng loại đường (**Line**) được chọn.

![007](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.4/007.png)

### Chúng ta đã làm gì?

Trước khi chúng ta tiếp tục, hãy xem lại lệnh *stats* trong truy vấn mới của chúng ta.

```sql
| stats sum(type="puppy") as puppy, sum(type="kitten") as kitten, sum(type="bunny") as bunny by bin(5m)
```

 - Chúng ta đã sử dụng một lệnh *stats* với hàm *sum*.
 - Trong hàm *sum*, chúng ta chỉ định cái gì để tổng hợp: tức là tất cả các sự kiện có giá trị loại là "puppy".
 - Chúng ta đã đặt tên cho kết quả tổng hợp, tức là puppy.
 - Chúng ta đã làm điều này 3 lần, một lần cho mỗi loại vật nuôi.
 - Chúng ta đã nhóm tổng hợp trên các khoảng thời gian 5 phút.

Việc sử dụng hàm *sum* thay vì hàm *count* là việc quan trọng. Chúng ta phải sử dụng *sum* thay vì *count* ở đây, vì điều kiện *type="puppy"* sẽ trả về 1 hoặc 0 cho mỗi sự kiện. *Count* sẽ đếm tất cả các số 1 và 0, về cơ bản đếm tất cả các sự kiện. *Sum* sẽ đếm những sự kiện có giá trị là 1 hoặc nơi type="puppy" là true.