---
title : "Storage"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

Regarding the storage, we will create a S3 Bucket in the N.Virginia region in order to store necessary files used for the workshop such as SQL query files to initialize Database server. 

Additionally, we will deploy a S3 Gateway Endpoint to connect to S3 Bucket from the VPC using AWS Backbone, avoiding sending requests through Internet.

![Storage](/images/additional/storage.png)

#### Content
- [Create S3 Bucket](2.2.1-creates3/)
- [Create Gateway Endpoint](2.2.2-creates3gatewayendpoint/)