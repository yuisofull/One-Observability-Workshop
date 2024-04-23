---
title : "Truy vấn thông qua AWS CLI"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 3.1.1.6 </b> "
---

Bạn cũng có thể truy vấn các log group sử dụng AWS CLI.

Những lệnh này có thể chạy từ môi trường Cloud9 của bạn.

{{% notice note %}}
Hãy xem phần về [Cài đặt Cloud9](2-Setup/2.1-cloud9/) nếu bạn chưa làm.
{{% /notice %}}

1. Thực thi truy vấn sau trong terminal:

Truy vấn dưới đây truy vấn 10 log records đầu tiên từ một log group trong một khoảng thời gian cụ thể.

```bash
aws logs start-query --log-group-name /ecs/PetListAdoptions --start-time $(date -d "12 hour ago" "+%s") --end-time $(date "+%s") --query-string 'fields @message | limit 10'
```

{{% notice note %}}
Bạn sẽ cần cập nhật các giá trị tham số thời gian bắt đầu và kết thúc đến các giá trị thời gian epoch chính xác. Bạn có thể tính toán các giá trị thời gian epoch từ trang web [này](https://www.epochconverter.com/).
{{% /notice %}}

Truy vấn trên sẽ trả về một queryId.

2. Thay thế *<QUERY_ID>* bằng ID truy vấn mà bạn đã sao chép từ kết quả và thực thi lệnh dưới đây trong terminal:

```bash
aws logs get-query-results --query-id <QUERY_ID>
```

Nếu không có log nào được trả về, hãy kiểm tra các giá trị thời gian epoch và thử lại.