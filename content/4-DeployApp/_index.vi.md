---
title : "Triển khai ứng dụng lên EC2 instance"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

Trong bước này, chúng ta sẽ tạo các phiên bản EC2 cho Frontend và Backend của ứng dụng nằm trong private subnet. Sau đó, chúng ta sẽ triển khai Load Balancer và Auto Scaling Group cho cả hai để đảm bảo khả năng mở rộng khi có sự tăng lên trong số lượng yêu cầu truy cập.

#### Content
- [Triển khai Online Shop Backend](4.1-createbackend/)
- [Tạo Application Load Balancer cho Backend](4.2-createalbbe/)
- [Triển khai Online Shop Frontend](4.3-createfrontend/)
- [Tạo Application Load Balancer cho Frontend](4.4-createalbfe/)
- [Tạo Auto Scaling Group cho Backend](4.5-createasgbe/)
- [Tạo Auto Scaling Group cho Frontend](4.6-createasgfe/)