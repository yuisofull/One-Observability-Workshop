---
title : "Truy vấn - sử dụng trường(fields) với regex"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 3.1.1.5 </b> "
---

Trong phần trước, chúng ta khám phá cách truy vấn với định dạng glob. Trong phần này, chúng ta sẽ xem cách sử dụng regex (biểu thức chính quy) để phân tách các trường của chúng ta.

Chúng ta vẫn đang tìm cách tạo một truy vấn cho phép chúng ta xem pattern của adoptions của các loại vật nuôi(pets) khác nhau theo thời gian.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.5/001.png)

### Sự kiện (events) trông như thế nào?

Các sự kiện mà chúng ta quan tâm đang ở trong nhóm log `/ecs/PayForAdoption`, và trông giống như thế này:

`POST /api/home/completeadoption?petId=024&petType=bunny HTTP/1.1 11.0.13.138:1876 200`

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.5/002.png)

Trước đó, sử dụng định dạng glob, chúng ta đã trích xuất các trường bằng cách sử dụng truy vấn sau:

```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message "* * * *:* *" as method, request, protocol, ip, port, status
| parse request "*?petId=*&petType=*" as requestURL, id, type
| display @timestamp, type
| limit 20
```

Tuy nhiên, chúng ta chỉ cần trường loại vật nuôi (pet type). Chúng ta có thể đã sử dụng regex (biểu thức chính quy) để rõ hơn ở đây.

Xem xét lệnh parse sau:

```sql
filter @message like /POST/ and @message like /completeadoption/
| parse @message /&petType=(?<type>\w+) HTTP/
| display @timestamp, type
| limit 20
```

Trước khi chúng ta khám phá lệnh này, hãy chạy nó và xem kết quả trông như thế nào.

1. Trong Bảng điều khiển Quản lý AWS trên menu Dịch vụ, nhấp vào **CloudWatch**.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào Logs **Insights**.

3. Từ menu thả xuống **Select log group(s)**, chọn nhóm log `/ecs/PayForAdoption`.

4. Thay thế truy vấn bằng truy vấn trên và **Run query**. Bạn nên thấy các sự kiện tương tự như được hiển thị ở trên.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.5/003.png)

Cùng đi qua định dạng regex trong lệnh parse.

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.5/004.png)

Để chỉ định dữ liệu chúng ta muốn nhận trong trường, chúng ta đã sử dụng **\w+**.

Trong regex điều này có nghĩa là:

- **\w** : Từ (a-z, A-Z, 0-9, _)
- **+** : 1 hoặc nhiều ký tự phù hợp. Có nhiều cách khác nhau để diễn chuỗi bạn muốn nhận.

{{% notice info %}}
**Thông tin thêm về biểu thức chính quy**\
Sử dụng regex là một cách mạnh mẽ để chỉ định và có được trường mong muốn. Chúng ta chỉ mới sử dụng một phần nhỏ của cú pháp ở đây.  \
\
Có nhiều tài nguyên hữu ích về cú pháp regex có sẵn trên web, ví dụ như [Cheatography cheatsheet](https://cheatography.com/davechild/cheat-sheets/regular-expressions/). Khám phá và tìm cái phù hợp với yêu cầu của bạn.
{{% /notice %}}

### Lấy nhiều trường

Bạn lấy thể bắt nhiều trường trong một biểu thức. Trong ví dụ dưới đây, chúng ta lấy petId và type. Lưu ý ký tự thoát (backslash) trước dấu hỏi trong *?petId=* (? có một ý nghĩa đặc biệt trong regex).

```sql
| parse @message /\?petId=(?<petId>\d+)&petType=(?<type>\w+) HTTP/
```