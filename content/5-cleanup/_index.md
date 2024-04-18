+++
title = "Clean up resources"
date = 2022
weight = 6
chapter = false
pre = "<b>5. </b>"
+++

### We will proceed with the following steps to delete the resources we created in this lab exercise.

#### Delete S3 bucket

1. Access the [S3 management console](https://s3.console.aws.amazon.com/s3/home)
   - Select the S3 bucket we created for this lab. (For example: bucket-for-lambda-55555)
   - Click **Empty**.
   - Enter **permanently delete**, then click **Empty** to delete objects in the bucket.
   - Click **Exit**.

2. After deleting all objects in the bucket, click **Delete**.

![Clean](/images/5.clean/001-clean.png)

3. Enter the S3 bucket name, then click **Delete bucket** to delete the S3 bucket.

![Clean](/images/5.clean/002-clean.png)

#### Delete API Gateway

1. Access the [API Gateway service management interface](console.aws.amazon.com/apigateway)
   - Select **S3 Upload**.
   - Click **Delete**.

![Clean](/images/5.clean/003-clean.png)

2. In the confirmation box, enter **confirm**.
   - Click **Delete** to delete the API Gateway.

#### Delete Lambda Function

1. Access the [Lambda service management interface](console.aws.amazon.com/lambda/home)
   - Click **s3-upload**.
   - Select **Actions**.
   - Click **Delete**.

![Clean](/images/5.clean/004-clean.png)

2. In the confirmation box, enter **delete** to confirm, then click **Delete** to delete the Lambda function and related resources.