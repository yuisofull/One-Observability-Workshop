---
title : "Truy vấn - sử dụng trường(fields)"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 3.1.1.4 </b> "
---

Chúng ta có một số lựa chọn về cách hiển thị kết quả truy vấn log của chúng ta.

Đôi khi, việc hiển thị đầu ra dưới dạng bảng dữ liệu sẽ hữu ích, còn đôi khi sẽ hữu ích hơn nếu cung cấp hiển thị trực quan dưới dạng biểu đồ thời gian, biểu đồ hình tròn, biểu đồ cột, hoặc biểu đồ khu vực chồng lên.

Chúng ta sẽ sử dụng một truy vấn đơn giản để đi qua từng lựa chọn hiển thị này và thêm chúng vào một bảng điều khiển để tạo ra bảng điều khiển được hiển thị ở đây.

![001](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/001.png)

### Chọn log group

1. Trên Bảng điều khiển quản lý AWS, trong menu *Services*, nhấp vào **CloudWatch**.

2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Logs Insights**.

3. Từ **Select log group(s)**, chọn */ecs/PetListAdoptions*.

### Tạo hiển thị dưới dạng bảng

4. Sử dụng truy vấn sau và chọn **Run query** để xem kết quả.
```sql
fields @timestamp, petcolor, pettype
| filter ispresent(petcolor) AND ispresent(pettype)
| stats count() by pettype
```
Bạn sẽ thấy một bảng kết quả.

![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/002.png)

5. Chọn **Add to dashboard**

Nếu bạn có sẵn một bảng điều khiển(dashboard), tiện ích (widget) có thể thêm vào bảng điều khiển hiện có. Ở đây, chúng ta sẽ tạo một bảng điều khiển mới.

6. Chọn **Create new**.

7. Đặt tên cho bảng điều khiển mới là `display-options` và **Create**.

8. Bạn có thể sửa tiêu đề ở đây (dưới ** Customize widget title**), hoặc sửa nó sau khi đã đặt trên bảng điều khiển. Tôi đã gọi tiện ích này là `table display`.

![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/003.png)

9. Chọn **Add to dashboard**.

10. Thay đổi kích thước tiện ích của bạn theo mong muốn và **Save dashboard**.

Bảng điều khiển của bạn sẽ trông giống như thế này. 

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/004.png)

### Hiển thị dưới dạng biểu đồ hình tròn

Chúng ta sẽ sử dụng lại truy vấn và thay đổi các tùy chọn hiển thị.

11. Từ CloudWatch dashboard của bạn, nhấp vào 3 chấm dọc ở phía trên bên phải của tiện ích biểu đồ thời gian đã tạo trước đó và chọn **Duplicate**. (Bạn có thể sao chép bất kỳ tiện ích nào theo cách này).

![005](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/005.png)

12. Trên tiện ích mới của bạn, nhấp vào 3 chấm dọc một lần nữa và chọn **Edit**. Bạn sẽ được chuyển về chế độ xem Insights Log để chỉnh sửa truy vấn của mình.

13. Chọn **Run query**.

{{% notice note %}}
Khi bạn chỉnh sửa một tiện ích, tab **Visualization** sẽ trống rỗng. Bạn phải **Run query** trước để có kết quả để hiển thị.
{{% /notice %}}

Truy vấn ở đây có kết quả ở định dạng có thể hiển thị theo nhiều cách.

14. Chọn tab **Visualization** và chọn **Pie** chart display.

15. Chọn **Save changes**.

16. Di chuột qua tiêu đề và nhấp vào biểu tượng chỉnh sửa (cây bút). Thay đổi tiêu đề thành `pie chart display`.

![006](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/006.png)

17. Thay đổi kích thước, vị trí và đổi tên tiện ích của bạn theo mong muốn. Nhớ lưu lại bảng điều khiển (**Save dashboard**).

Bảng điều khiển của bạn sẽ trông giống như thế này. 

![007](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/007.png)

### Hiển thị dưới dạng biểu đồ cột

18. Sao chép(duplicate) một trong các tiện ích(widgets) của bạn như trước, sau đó chọn **Edit** và **Run query**.

19. Chọn tab **Visualization** và chọn **Bar** chart display.

20. Chọn **Save changes**.

21. Di chuột qua tiêu đề và nhấp vào biểu tượng chỉnh sửa (cây bút). Thay đổi tiêu đề thành `bar chart display`.

22. Thay đổi kích thước, vị trí và đổi tên tiện ích của bạn theo mong muốn. Nhớ lưu lại bảng điều khiển (**Save dashboard**).

Bảng điều khiển của bạn sẽ trông giống như thế này.

![008](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/008.png)

### Hiển thị dưới dạng biểu đồ thời gian

Biểu đồ thời gian và biểu đồ stacked area yêu cầu dữ liệu đã được tổng hợp theo thời gian.

Trong CloudWatch, chúng tôi gọi biểu đồ thời gian là một biểu đồ đường.

Vì sự đơn giản, chúng tôi chỉ sẽ xem xét thêm một loạt dữ liệu nhất định vào các biểu đồ này.

23. Sao chép(duplicate) một trong các tiện ích của bạn như trước, sau đó chọn **Edit**.

24.Chỉnh sửa lệnh stats trong truy vấn của bạn để bao gồm một tổng hợp thời gian như dưới đây và **Run query**.

```sql
fields @timestamp, petcolor, pettype
| filter ispresent(petcolor) AND ispresent(pettype)
| stats count() by bin(5m)
```

25. Chọn tab **Visualization** và chọn hiển thị **Line** chart display.

26. Chọn **Save changes**.

27. Di chuột qua tiêu đề và nhấp vào biểu tượng chỉnh sửa (cây bút). Thay đổi tiêu đề thành `time chart display`.

28. Thay đổi kích thước, vị trí và đổi tên tiện ích của bạn theo mong muốn. Nhớ lưu lại bảng điều khiển (**Save dashboard**).

Bảng điều khiển của bạn sẽ trông giống như thế này. 

![009](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/009.png)

### Hiển thị dưới dạng stacked area

29. Sao chép(duplicate) một trong các tiện ích của bạn như trước, sau đó chọn **Edit** và **Run query**.

{{% notice note %}}
Nếu bạn nhìn thấy một lỗi chỉ ra rằng dữ liệu không phù hợp cho biểu đồ đường (line chart), bạn sẽ cần chỉnh sửa truy vấn để bao gồm một tổng hợp thời gian như đã làm cho hiển thị biểu đồ thời gian.
{{% /notice %}}

30. Chọn tab **Visualization** và chọn hiển thị **Stacked area** chart display.

31. Chọn **Save changes**.

32. Di chuột qua tiêu đề và nhấp vào biểu tượng chỉnh sửa (cây bút). Thay đổi tiêu đề thành `stacked area display`.

33. Thay đổi kích thước, vị trí và đổi tên tiện ích của bạn theo mong muốn. Nhớ lưu lại bảng điều khiển (**Save dashboard**).

Bảng điều khiển của bạn sẽ trông giống như thế này.

![010](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/010.png)


{{% notice note %}}
**Biểu đồ thời gian với nhiều loạt dữ liệu** \
![011](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.3/011.png)
Ví dụ trên được tạo ra dưới dạng biểu đồ đường trong phần của workshop về [Truy vấn - sử dụng trường](3-Native_observe/3.1-Logs/3.1.1-LogInsights/3.1.1.4-Querying_using_fields/).\
Nếu bạn muốn khám phá cách vẽ đồ thị nhiều loạt dữ liệu trên một biểu đồ thời gian, bạn có thể xem một ví dụ trong phần [Truy vấn - sử dụng trường](3-Native_observe/3.1-Logs/3.1.1-LogInsights/3.1.1.4-Querying_using_fields/).
{{% /notice %}}