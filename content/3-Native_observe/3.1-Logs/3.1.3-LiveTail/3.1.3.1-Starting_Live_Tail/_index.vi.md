---
title : "Bắt đầu Live Tail"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1.3.1 </b> "
---

Bạn có thể truy cập tính năng **Live Tail** trực tiếp từ menu của AWS CloudWatch console ở phần **Logs**. Từ đây, bạn có thể chọn nhóm log và có thể từ stream log cụ thể để bao gồm trong phiên của Live Tail.

Bạn cũng có thể bắt đầu một phiên trail từ một nhóm log cụ thể:

1. Nhấp vào **Log groups** trong phần **Logs** trên thanh điều hướng bên trái của menu.

2. Chọn ô đánh dấu cho Log group */aws/containerinsights/PetSite/application*, nút **Start Tailing** sẽ trở nên khả dụng.

3. Nhấp vào nút **Start Tailing**.

![001](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/001.png)

{{% notice note %}}
**Chú ý** \
Để khởi tạo một phiên live tail cho một stream  log cụ thể trong một nhóm log, làm các bước sau:
\
\
1.Nhấp vào **Log groups** trong phần **Logs** trên thanh điều hướng bên trái của menu. \
2.Nhấp vào tên của một Log Group.\
3.Cuộn xuống tab Log streams, và nhấp vào tên của một stream log.\
4.Nhấp vào nút **Start Tailing**.
![002](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/002.png)
{{% /notice %}}

### Filtering configuration panel

Khi Live Tail console bắt đầu, bạn nhấp vào biểu tượng **Filter**, bạn sẽ thấy Filter configuration panel được mở rộng hiển thị các tùy chọn filter có sẵn:

![003](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/003.png)

1. Chọn một nhóm log - điều này được điền sẵn nếu method đầu tiên được sử dụng. Để bắt đầu theo dõi các events trực tiếp, ít nhất một nhóm log phải được chọn. Tất cả các input field và các nút sẽ chỉ được kích hoạt sau khi log group selection được điền. Đối với workshop này, đảm bảo nhóm log có tên */aws/containerinsights/PetSite/application* được chọn.

2. Chọn log stream [optional] - tùy chọn này chỉ có sẵn khi chỉ có một nhóm log được chọn. Nó bị vô hiệu hóa khi hơn một nhóm log được chọn. Nếu không chọn gì cả, tất cả các log stream trong nhóm log đã chọn sẽ được thêm vào. Có hai cách để tìm log stream:
  + Theo Tên - nhấp vào danh sách thả xuống để xem danh sách các log stream và chọn từng cái, hoặc, gõ một tên để tìm kiếm và chọn.
  + Theo Prefix - Các log stream có thể được chọn dựa trên một prefix chung.

 Đối với workshop này, hãy để filter trống để bao gồm tất cả các log stream.

3. Thêm filter patterns [optional] - tùy chọn này phân biệt chữ hoa và cho phép lọc dựa trên nội dung của các thông điệp trong các nhóm log và log stream đã chọn. Đối với workshp[] này, sử dụng giá trị này để lấy chỉ các logý từ bên trong *petsite* container:

    ```
    { $.kubernetes.container_name = "petsite" }
    ```

{{% notice info %}}
  Tìm hiểu thêm về [filter patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html#matching-terms-events).
{{% /notice %}}

- Bắt đầu theo dõi - sau khi tiêu chí lọc(filter) mong muốn được thực hiện, nhấp vào **Apply filters** để bắt đầu phiên theo dõi và hiển thị các events trực tiếp mới nhất khi chúng đang stream vào AWS CloudWatch Logs.

### Các nút và lựa chọn của console

Bạn có thể nhập tối đa 5 terms để được highlight trong phiên live tail. Các term không phân biệt chữ hoa và bạn sẽ thấy chúng trong cửa sổ phiên tail. Nhập mỗi từ sau đây sau đó nhấn Enter (không tách ra bằng dấu cách):

- `dns`
- `udp`
- `recorder`
- `log`
- `failed`

![004](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/004.png)

Khi filter được áp dụng, bạn sẽ thấy các thanh màu ở bên trái của mỗi dònge vent, xác định từ khóa nào được tìm thấy.

Mở rộng một event có mã màu bên cạnh, và nó sẽ hiển thị chính xác vị trí của event chứa từ khóa.

![005](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/005.png)

Sử dụng biểu tượng kính lúp nằm kế bên event để mở cửa sổ mới cho phép bạn xem xét nó và sao chép nội dung vào bảng ghi tạm của bạn. Bạn cũng có thể truy cập nguồn cụ thể của thông điệp này từ đây qua liên kết *View trailing events*.

![006](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/006.png)

{{% notice note %}}
**Chú ý** \
Các term được highlight khác biệt so với các Filter Keywords trong việc filter sử dụng để thu hẹp lại các event được lọc ra và hiển thị, trong khi các highlighted terms giúp xác định một cách chính xác những gì bạn đang tìm kiếm. Ví dụ, một developer có thể chỉ muốn xem các sự kiện có mức độ 'error' (Filter keyword), tuy nhiên, họ cũng có thể quan tâm hơn đến mã lỗi *HTTP 504* (highlight term).
{{% /notice %}}

(xx events/s, % displayed) - Khi số lượng event mỗi giây stream đến console ở mức chấp nhận được, số 'events/s' sẽ phản ánh số event được lọc thực sự và '%displayed' sẽ hiển thị 100%. Tuy nhiên, nếu số lượng event cao hơn dự kiến, một phần trăm event có thể bị bỏ qua. Ví dụ '100 events/s, 60% displayed' chỉ ra rằng 167 event đã được filter nhưng chỉ có 100 (rate tối đa) được truyền đến console và số còn lại là 67 bị bỏ qua.

{{% notice note %}}
**Chú ý** \
Các log events bị bỏ qua được loại bỏ khỏi Live Tail console nhưng vẫn tiếp tục được xử lý và lưu vào CloudWatch Logs.
{{% /notice %}}

### Events Window

Hiển thị các event đang được stream và cho phép bạn tạm dừng và xem chi tiết của một event bằng cách nhấp bất kỳ đâu trong Events Window. Khi event stream bị tạm dừng, nhấp vào nút **Resume session** (góc dưới bên phải) sẽ ngay lập tức hiển thị các event trực tiếp mới nhất.

![007](/images/3.native_observe/3.1-logs/3.1.3/3.1.3.1/007.png)

Nhấp vào **Cancel** để dừng việc theo dõi các event.