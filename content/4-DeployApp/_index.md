---
title : "Deploy application onto EC2 Instances"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

In this step, we will create EC2 instances for Frontend and Backend of the application located in the private subnet. Then, we will deploy Load Balancers and Auto Scaling Group for both of them to ensure their scalability in the event of increasing in the number of request access.

#### Content
- [Deploy Online Shop Backend](4.1-createbackend/)
- [Create Application Load Balancer Backend](4.2-createalbbe/)
- [Deploy Online Shop Frontend](4.3-createfrontend/)
- [Create Application Load Balancer Frontend](4.4-createalbfe/)
- [Create Auto Scaling Group for Backend](4.5-createasgbe/)
- [Create Auto Scaling Group for Frontend](4.6-createasgfe/)