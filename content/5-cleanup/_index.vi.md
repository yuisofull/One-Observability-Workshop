+++
title = "Dọn dẹp tài nguyên  "
date = 2021
weight = 6
chapter = false
pre = "<b>5. </b>"
+++

Chúng ta sẽ tiến hành các bước sau để xóa các tài nguyên chúng ta đã tạo trong bài thực hành này.

#### Xóa S3 bucket

1. Truy cập [giao diện quản trị dịch vụ S3](https://s3.console.aws.amazon.com/s3/home)
  + Click chọn S3 bucket chúng ta đã tạo cho bài thực hành. ( Ví dụ : bucket-for-lambda-55555 )
  + Click **Empty**.
  + Điền **permanently delete**, sau đó click **Empty** để tiến hành xóa object trong bucket.
  + Click **Exit**.

2. Sau khi xóa hết object trong bucket, click **Delete**

![Clean](/images/5.clean/001-clean.png)

3. Điền tên S3 bucket, sau đó click **Delete bucket** để tiến hành xóa S3 bucket.

![Clean](/images/5.clean/002-clean.png)

#### Xóa API Gateway

1. Truy cập vào [giao diện quản trị dịch vụ API Gateway](console.aws.amazon.com/apigateway)
  + Chọn **S3 Upload**
  + Chọn **Delete**

![Clean](/images/5.clean/003-clean.png)

2. Tại ô confirm , điền **confirm**.
  + Click **Delete** để tiến hành xóa API Gateway.

#### Xóa Lambda Function

1. Truy cập vào [giao diện quản trị dịch vụ Lambda](console.aws.amazon.com/lambda/home)
  + Click **s3-upload**.
  + Click chọn **Actions**.
  + Click **Delete**.

![Clean](/images/5.clean/004-clean.png)

2. Tại ô confirm, điền **delete** để xác nhận, click **Delete** để thực hiện xóa **Lambda function** và các tài nguyên liên quan.

