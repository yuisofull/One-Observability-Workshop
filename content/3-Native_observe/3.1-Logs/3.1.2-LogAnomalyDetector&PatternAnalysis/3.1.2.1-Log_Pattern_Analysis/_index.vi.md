---
title : "Phân Tích Log Pattern"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1.2.1 </b> "
---

CloudWatch Logs Insights sử dụng các thuật toán machine learning để tìm **Patterns** khi bạn truy vấn logs của mình. Một pattern là một cấu trúc văn bản chung xuất hiện liên tục trong các log fields của bạn. Các pattern hữu ích để phân tích các tập hợp log lớn vì một số lượng lớn các log event thường có thể được nén thành một số ít pattern.

1. Trong Bảng điều khiển quản lý AWS, trên menu Services, nhấp vào **CloudWatch**.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Logs Insights**.

3. Chọn *log group* có tên */ecs/PetListAdoptions*, */ecs/PayForAdoption*, */ecs/PetSearch*, */ecs/PetTrafficGenerator* từ **Select log group(s)**.

Bạn sẽ thấy một truy vấn mẫu được tự động đặt trong trường truy vấn.

4. Chúng ta muốn chọn tất cả các sự kiện trong nhóm log trong **3h**, vì vậy hãy thay đổi giới hạn thành **2000**.

5. Nhấp vào **Run query**.

![001](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.1/001.png)

6. Bây giờ chuyển sang Tab **Patterns** bên cạnh Logs Insights

{{% notice info %}}
Insight: Tab **Patterns** trên trang Logs Insights tìm các patterns trong kết quả truy vấn của bạn và cho phép bạn phân tích chúng chi tiết. Điều này làm cho việc tìm kiếm dễ dàng hơn và đi sâu vào nội dung lạ hoặc không mong muốn trong nhật ký của bạn.
{{% /notice %}}

![002](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.1/002.png)

**Event Count** cho chúng ta biết số lần mà pattern xuất hiện trong các log events đã truy vấn. Chúng ta có thể nhấp vào cột này để sắp xếp các mẫu theo tần suất.

**Event Ratio** là phần trăm (%) của log events đã truy vấn chứa pattern này.

**Severity Type**, sẽ là một trong các loại sau: **ERROR** nếu mpattern chứa từ **ERROR** và **INFO** nếu pattern không chứa từ **WARN** hoặc **ERROR**.

7. Bây giờ để tập trung vào các errors, chúng ta có thể *filter* các *Patterns* theo ERROR, để làm điều đó, chúng ta có thể đi đến phần Filter patterns và chọn **Severity Type** trong menu thả xuống và chọn **Severity Type = ERROR**

![003](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.1/003.png)

8. Bạn có thể kiểm tra các mẫu này để có hiểu biết sâu hơn về pattern này, bằng cách nhấp vào nút **Inspect** cho **first pattern** được hiển thị ở trên.

![004](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.1/004.png)

{{% notice info %}}
Insight: Các trường(fields) trong một pattern được gọi là **tokens**. Các trường có sự thay đổi trong một pattern, chẳng hạn như một request ID hoặc timestamp, được gọi là **dynamic tokens**.
{{% /notice %}}

**Histogram** thể hiện số lần xuất hiện của pattern trong phạm vi thời gian đã truy vấn, trong trường hợp của chúng ta là *3h*.

Tab **Log samples** hiển thị một số log event mà phù hợp với pattern được chọn.

Tab **Token Values** hiển thị các giá trị của dynamic toekn được chọn, nếu bạn đã chọn một.

9. Bạn có thể đóng tab **Pattern Inspect** sau khi đã chẩn đoán được vấn đề!