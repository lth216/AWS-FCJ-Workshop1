---
title : "Storage"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

Về phần lưu trữ, ta sẽ tạo 1 S3 Bucket ở vùng N.Virginia để lưu trữ những file cần thiết dùng cho workshop như là file cấu hình Database.

Thêm vào đó, ta sẽ tạo S3 Gateway Endpoint ở VPC để sử dụng AWS backbone network, hạn chế việc truy xuất thông tin thông qua Internet.

![Storage](/images/additional/storage.png)

#### Nội dung
- [Tạo S3 Bucket](2.2.1-creates3/)
- [Tạo S3 Gateway Endpoint](2.2.2-creates3gatewayendpoint/)