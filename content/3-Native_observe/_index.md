---
title : "Upload file"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

### In this step, we will upload an image to S3 using **curl** or **Postman**

#### Curl

Open your terminal and enter:
```
curl -v --request POST '{Invoke URL from previous step}/upload' --form file='@{Path to the image}'
```
For example, in my case it would be:
```
curl -v --request POST 'https://4x87mid1b8.execute-api.ap-southeast-1.amazonaws.com/staging/upload' --form file='@C:\Users\Admin\ok.png'
```
If successful, it will return the link at the end like this:
```
{"link":"https://lambda-test-12333.s3.ap-southeast-1.amazonaws.com/ok-1708837492785249474.png"}* Connection #0 to host 4x87mid1b8.execute-api.ap-southeast-1.amazonaws.com left intact
```

#### Postman

+ In the **body** section, select **form-data**.
    + Add **key** as **file**
        + Select the file you want as the **value**

![upload](/images/3.upload/001-upload.png)

After successful upload, you can see the uploaded file in your bucket.

![upload](/images/3.upload/002-upload.png)