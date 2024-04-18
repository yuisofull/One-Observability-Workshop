---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Serverless

Serverless is a cloud development model that allows you to build and run your applications easily without managing any servers.

Here are four benefits I see in the Serverless model:
- Reducing the cost of managing and operating servers, which is often the responsibility of DevOps, and this workload can be reduced. For example, tasks like creating, managing, and monitoring EC2 instances.
- Automatic scaling and high availability: Our Functions as a Service (FaaS) will automatically scale with traffic, requiring minimal configuration unless we want it to scale in a specific way.
- Optimizing cloud resource usage: The most important issue with the cloud is the monthly cost, and when we use FaaS, we only need to pay for each triggered function.
- Supporting multiple languages: We can use various programming languages to write FaaS.

### AWS Lambda

Lambda is a serverless compute service that allows you to run applications without provisioning or managing servers. Lambda runs on a highly available platform and manages all compute resources, including server and operating system maintenance, capacity provisioning, automatic scaling, and logging. With Lambda, you can develop almost any type of application or backend service.

You organize your application into Lambda functions. Lambda functions only run when needed and have the ability to automatically scale, from a few requests per day to thousands per second. You only pay for the compute time you consume - AWS does not charge when your application is not running.

In this hands-on exercise, we will help users upload files to a bucket using a POST request through API Gateway and Lambda Function.