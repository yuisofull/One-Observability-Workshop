---
title : "Thiết lập Phát hiện Bất thường trên CloudWatch Logs"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.1.2.3 </b> "
---

Chúng ta sẽ kích hoạt bộ phát hiện bất thường liên tục quét các log events trong nhóm log và tìm ra các bất thường trong dữ liệu log. Chúng ta sẽ sử dụng các nhóm log */ecs/PetListAdoption* và */ecs/PetSearch* làm ví dụ.

1. Trên menu Dịch vụ trong AWS Management Console, nhấp vào **CloudWatch**.
2. Trong menu điều hướng bên trái dưới **Logs**, nhấp vào **Log Anomalies**.

![001](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.3/001.png)

3. Nhấp vào **Create anomaly detector**.

Bạn sẽ thấy trang cấu hình cho Anomaly detection.

4. Chọn log group được gọi là */ecs/PetListAdoptions* từ danh sách chọn Log group.

{{% notice note %}}
Chỉ một log group được chọn khi tạo một anomaly detector(bộ phát hiện bất thường).
{{% /notice %}}

5. Nhập `petlist-adoptions-anomaly-detector` cho *Anomaly detector name*.

6. Chọn `5 min` cho *Evaluation frequency*.

![002](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.3/002.png)

Trong menu thả xuống *Filter patterns*, bạn có thể nhập các filter patterns mà machine learning model sẽ đánh giá như các error patterns. Hiện tại, chúng ta sẽ không cấu hình bất kỳ pattern nào, vì vậy hãy để trống chỗ này.

![003](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.3/003.png)

7. Nhấp vào **Activate anomaly detection**.

Bây giờ, *Anomaly Detector* sẽ được kích hoạt cho nhóm log. Một model sẽ bắt đầu training trên dữ liệu log và tạo ra một cơ sở để nhận biết bất kỳ pattern nào không bình thường.

{{% notice info %}}
**Quá trình training log detector sẽ mất bao lâu?** \
Lưu ý: Quá trình training thường mất khoảng 24 giờ để hoàn thành, vì vậy các Anomalies (Bất thường) có thể không xuất hiện ngay lập tức.
{{% /notice %}}

![004](/images/3.native_observe/3.1-logs/3.1.2/3.1.2.3/004.png)

8, Bây giờ lặp lại các bước 2-7 để tạo một bộ phát hiện bất thường khác, nhưng lần này cho */ecs/PetSearch*. Đối với Anomaly detector name, nhập `petsearch-adoptions-anomaly-detector` vào cấu hình Anomaly detector và chọn cùng một giá trị tần suất là `5 min`.