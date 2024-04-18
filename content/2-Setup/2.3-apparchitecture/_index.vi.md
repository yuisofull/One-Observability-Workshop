---
title : "Kiến trúc ứng dụng"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

Biểu đồ sau minh họa các thành phần khác nhau của ứng dụng PetAdoptions. Đến thời điểm này, bạn nên đã triển khai ứng dụng PetAdoption trên tài khoản AWS bạn. Hãy nhớ xóa tất cả các tài nguyên bằng cách tuân theo [hướng dẫn về dọn dẹp](5-cleanup/) để tránh các chi phí sử dụng không cần thiết.

Nội dung của workshop này chủ yếu sử dụng kiến trúc sau đây:
![001](/images/2.setup/2.3-apparchitecture/001.png)

## Khám phá ứng dụng (optional)

### Option 1: sử dụng Cloud9 (hoặc AWS CLI)

{{% notice info %}}
Bạn cần hoàn thành [các bước cài đặt Cloud9](2-setup/2.1-cloud9/) trước đó, nếu không, hãy hướng tới *Option 2*.
{{% /notice %}}

Chạy câu lệnh sau:
```bash
aws ssm get-parameter --name '/petstore/petsiteurl'  | jq -r .Parameter.Value
```

### Option 2: sử dụng CloudFormation console

Ở phía trong **CloudFormation** console bạn cần làm:
1. Chọn **Stacks** ở thanh điều hướng bên trái.
2. Kiếm stack tên là **Services** và click nó.
3. Chọn tab **Outputs**, bạn sẽ thấy URL website xuất hiện ngay ở **PetSiteUrl**:

![002](/images/2.setup/2.3-apparchitecture/002.png)

### Bước tiếp theo

Mở link ở một trang web mới, bạn sẽ thấy ứng dụng như bức ảnh dưới đây:
![003](/images/2.setup/2.3-apparchitecture/003.png)

### Troubleshooting

Trong trường hợp hiếm có, có khả năng bạn sẽ gặp phải vấn đề website không hiện bất kì hình ảnh nào, chọn *Perform Housekeeping* ở ngay trên góc phải của website.