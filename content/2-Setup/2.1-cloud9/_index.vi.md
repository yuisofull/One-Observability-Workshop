---
title : "Cài đặt môi trường Cloud9"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

Chúng ta sẽ sử dụng CloudShell để tạo Cloud9 instance cho việc triển khai ứng dụng PetAdpotions.

### ĐiềU kiện tiên quyết

- Trong hướng dẫn này chúng ta sẽ sử dụng CloudShell, nếU bạn không muốn sử dụng CloudShell, bạn phải cài đặt và có sẵn AWS CLI trên máy của bạn và thực hiện các câu lệnh trong các bước sắp tới.

- Profile của bạn phải có quyền admin.

### Setup

1. Trong AWS Management Console ở trên Services, chọn *CloudShell*.
![001](/images/2.setup/2.1-cloud9/001.png)
2. Sao chép và dán đoạn lệnh sau vào terminal: 
```bash
curl -O https://raw.githubusercontent.com/aws-samples/one-observability-demo/main/cloud9-cfn.yaml

aws cloudformation create-stack --stack-name C9-Observability-Workshop --template-body file://cloud9-cfn.yaml --capabilities CAPABILITY_NAMED_IAM

aws cloudformation wait stack-create-complete --stack-name C9-Observability-Workshop

echo -e "Cloud9 Instance is Ready!!\n\n"
```

4. Sau khoảng thời gian ngắn, terminal sẽ xuất hiện *"Cloud9 Instance is Ready!!*. Điều này có nghĩ là môi trường Cloud9 của bạn đã sẵn sàng 