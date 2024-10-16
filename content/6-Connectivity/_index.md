---
title : "Mitigate latency with CloudFront distribution"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

In this section, we will deploy CloudFront to speed up distribution of our static and dynamic web content to users. CloudFront routes the user request through AWS backbone network to the edge location that can best serve our content, reducing the number of networks that the user's requests must pass through, and therefore enhancing the performance of the application. Since our strategy is to improve the security of the web application, we will configure CloudFront to require HTTPS both to communicate with viewers and to communicate with the origin (in this case is Frontend Load Balancer).
At the end of the day, we will replace the DNS domain of CloudFront with our own domain (created in step 2) which is more friendly and memorable.

#### Content
- [Request SSL certificate with AWS ACM](6.1-createacm/)
- [Create CloudFront distribution](6.2-createcloudfront/)