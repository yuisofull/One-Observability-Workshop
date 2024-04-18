---
title : "Triển khai"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.2 </b> "
---

{{% notice info %}}
Bài workshop này cực kì khuyến khích bạn sử dụng Cloud9 để cài đặt cũng như tương tác. Trong khi bạn đang sử dụng terminal của chính bạn, những bước ở đây đã được tối ưu hoá và hoạt động với Cloud9. Nếu bạn không sử dụng Cloud9, workshop này sẽ coi như bạn có phương pháp thay thế phù hợp.
{{% /notice %}}

### Chỉnh sửa quyền IAM cho Cloud9

1. Ở AWS Management Console trên phần Services, kiếm và chọn Cloud9.

2. Click **Open** trên **observabilityworkshop**.
![001](/images/2.setup/2.2-deploy/001.png)

3. Chọn vào icon hình bánh răng trên góc phải để mở tab **Preferences**.
 - Chọn **AWS SETTINGS** ở thanh menu bên trái.
 - Tắt cài đặt **AWS managed temporary credentials**
 - Đóng tab **Preferences**.
![002](/images/2.setup/2.2-deploy/002.png)

4. Điều hướng tới terminal ở dưới. (Nếu bạn không thấy terminal, chọn **Window** ở phía bên trên, chọn **New Terminal**)

### Xoá credentials tạm thời

5. Chạy câu lệnh sau trong terminal:
```bash
rm -vf ${HOME}/.aws/credentials
```
{{% notice note %}}
Câu lệnh trên đơn giản để đảm bảo rằng không còn credentials tạm thời nào bằng cách xoá files chứa credentials đi
{{% /notice %}}

### Cài đặt AWS CLI với AWS Region hiện tại là mặc định:
6. Chạy câu lệnh sau trong terminal:
```bash
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region
```
{{% notice note %}}
Đoạn lệnh trên cài đặt AWS CLI sử dụng AWS Region hiện tại là mặc định
{{% /notice %}}

### Kiểm tra cài đặt môi trường Cloud9

7. Chạy câu lệnh sau trong terminal:

```bash
test -n "$AWS_REGION" && echo AWS_REGION is "$AWS_REGION" || echo AWS_REGION is not set

aws sts get-caller-identity --query Arn | grep observabilityworkshop-admin -q && echo "You're good. IAM role IS valid." || echo "IAM role NOT valid. DO NOT PROCEED."
```
{{% notice note %}}
Đoạn lệnh trên kiểm tra xem cài đặt môi trường của bạn có chính xác không.
{{% /notice %}}

{{% notice warning %}}
Khi sử dụng Cloud9, **hãy chắc chắn rằng IAM role hợp lệ**. Nếu không hãy qua lại và xác minh lại các bước trong trang này.
{{% /notice %}}

### Cài đặt CDK

8. Nếu repo chưa được clone, clone repo sử dụng câu lệnh:
```bash
git clone https://github.com/aws-samples/one-observability-demo
```
Sau khi clone thành công, chạy câu lệnh sau trong terminal:
```bash
cd one-observability-demo/PetAdoptions/cdk/pet_stack
npm install
```
{{% notice note %}}
Đoạn lệnh trên di chuyển tới folder *pet_stack* và cài đặt các npm packages.
{{% /notice %}}

### Bootstrap CDK

9. Chạy câu lệnh sau để cài đặt Bootstrap CDK (hãy chắc chắn rằng bạn đang ở folder **pet_stack**):
```bash
cdk bootstrap
```

### Trao quyền truy cập tới EKS Console (optional)

10. Chạy câu lệnh sau (optional):

  - **Lưu ý**: hãy thay **\<Enter your Role ARN\>** với ARN của AWS Identity của bạn.

{{% notice note %}}
EKS Console mới gần đây ra mắt trong AWS. Để toàn quyền truy cập tới Console mới, một số quyền cần được trao ở trong EKS Cluster RBAC như miêu tả ở [đây](https://docs.aws.amazon.com/eks/latest/userguide/view-workloads.html). Câu lệnh dưới đây trao quyền truy cập tới EKS Console.
{{% /notice %}}
 - Workshop này khuyến khích bạn để vào **Role ARN** bạn dùng để truy cập AWS Console cũng như có truy cập tới Amazon EKS service.
```bash
CONSOLE_ROLE_ARN=<Enter your Role ARN>
```

### Triển khai ứng dụng 

11. Chạy các câu lệnh sau trên terminal để triển khai ứng dụng PetAdoptions trên tài khoản của bạn (hãy chắc chắn rằng bạn đang ở folder **pet_stack**):
```bash
EKS_ADMIN_ARN=$(../../getrole.sh)

echo -e "\nRole \"${EKS_ADMIN_ARN}\" will be part of system\:masters group\n" 

if [ -z $CONSOLE_ROLE_ARN ]; then echo -e "\nEKS Console access will be restricted\n"; else echo -e "\nRole \"${CONSOLE_ROLE_ARN}\" will have access to EKS Console\n"; fi

cdk deploy --context admin_role=$EKS_ADMIN_ARN Services --context dashboard_role_arn=$CONSOLE_ROLE_ARN --require-approval never

cdk deploy Applications --require-approval never

```

Nó sẽ mất vài phút để ứng dụng được triển khai hoàn toàn.

### Cập nhật kubeconfig

12. Chạy câu lệnh sau trong temrinal để cập nhật **kubeconfig** giúp bạn tương tác với **EKS cluster**:

```bash
aws eks update-kubeconfig --name PetSite --region $AWS_REGION            
kubectl get nodes                                
```

Sau khi chạy, nó nên hiện ra như thế này
![003](/images/2.setup/2.2-deploy/003.png)

### Tổng kết

Đây là tất cả lệnh bạn chạy trong phần này, để nhanh gọn, bạn có thể sao chép hết lệnh dưới đây và chạy trong terminal:

```bash
rm -vf ${HOME}/.aws/credentials

export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
echo "export ACCOUNT_ID=${ACCOUNT_ID}" | tee -a ~/.bash_profile
echo "export AWS_REGION=${AWS_REGION}" | tee -a ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region

test -n "$AWS_REGION" && echo AWS_REGION is "$AWS_REGION" || echo AWS_REGION is not set

aws sts get-caller-identity --query Arn | grep observabilityworkshop-admin -q && echo "You're good. IAM role IS valid." || echo "IAM role NOT valid. DO NOT PROCEED."

git clone https://github.com/aws-samples/one-observability-demo

cd one-observability-demo/PetAdoptions/cdk/pet_stack

npm install

# CONSOLE_ROLE_ARN=<Enter your Role ARN>

EKS_ADMIN_ARN=$(../../getrole.sh)

echo -e "\nRole \"${EKS_ADMIN_ARN}\" will be part of system\:masters group\n" 

if [ -z $CONSOLE_ROLE_ARN ]; then echo -e "\nEKS Console access will be restricted\n"; else echo -e "\nRole \"${CONSOLE_ROLE_ARN}\" will have access to EKS Console\n"; fi

cdk deploy --context admin_role=$EKS_ADMIN_ARN Services --context dashboard_role_arn=$CONSOLE_ROLE_ARN --require-approval never

cdk deploy Applications --require-approval never

aws eks update-kubeconfig --name PetSite --region $AWS_REGION            
kubectl get nodes                                
```
