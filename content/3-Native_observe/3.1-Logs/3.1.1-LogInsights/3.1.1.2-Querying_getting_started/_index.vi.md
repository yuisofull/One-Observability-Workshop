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

5. Ở menu phía bên phải, nhấp **Discovered fields**

Bạn sẽ thấy một danh sách các trường được tìm thấy bởi CloudWatch một cách tự động:
![002](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/002.png)


{{% notice note %}}
**Trường (fields) bắt đầu với "@"** 
Trường (fields) bắt đầu với "@" là các trường được [CloudWatch tự động tạo ra](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_AnalyzeLogData-discoverable-fields.html).

Trường *@message* chứa log raw(chưa được xử lí hay parse)
{{% /notice %}}

Việc này cho phép bạn chọn lấy những trường bạn muốn sử dụng trong truy vấn với intellisense:
![003](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/003.png)

Những trường được tự động tìm thấy bởi vì chúng ở dạng JSON.

{{% notice note %}}
**Định dạng JSON** 
Định dạng JSON là sự lựa chọn tuyệt vời cho dữ liệu log, CloudWatch hỗ trợ khả năng tự động tìm thấy các trường trong JSON. Do đó bạn có thể dễ dàng sử dụng chúng trong các truy vấn của bạn.
Tham khảo thêm ở [Fields in JSON logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_AnalyzeLogData-discoverable-fields.html).
{{% /notice %}}

### Các mẫu truy vấn

Log Insights cung cấp các mẫu truy vấn giúp bạn dễ dàng bắt đầu với chúng.

6. Ở menu phía bên phải, nhấp **Queries**.

Bạn sẽ thấy một danh sách các mẫu truy vấn được sắp xếp theo thể loại như theo dịch vụ hay theo tần suất sử dụng.

Chọn một trong số chúng và nhấp **Apply**, truy vấn sẽ được khởi tạo và bạn có thể chạy chúng.

- ***Lưu ý***: nó có thể không trả về kết quả cho các log group bạn đã chọn vì nó cho rằng trả về định dạng dữ liệu cơ bản.

![004](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/004.png)

7. Sao chép và dán truy vấn sau vô editor.

```sql
fields @timestamp, pettype
| filter ispresent(pettype)
| stats count() by pettype
```

8. Nhấp **Run query** để thấy nó trả về kết quả.

9. Nhấp **Save** ở dưới editor.

10. Nhập `Sample1` cho **Query name**.

11. Chọn **Save**.

![005](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/005.png)

12. Ở thanh menu điều hướng bên phải, chọn **Queries**.

Bạn nên thấy một truy vấn tên là *Sample1* ở phần **Saved queries**.

![006](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/006.png)

### Lịch sử truy vấn

13. Nhấp nút **History** ở dưới editor.

Ở đây bạn có thể thấy lịch sự tất cả truy vấn đã thực hiện. Bạn sẽ thấy truy vấn được thực thi bởi người đăng nhập vô cho dù họ có lưu lại hay không.

![007](/images/3.native_observe/3.1-logs/3.1.1-log_insight/3.1.1.1-interface/007.png)