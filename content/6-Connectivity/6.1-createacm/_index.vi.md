---
title : "Tạo chứng chỉ SSL/TLS với AWS ACM"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

#### Tạo chứng chỉ SSL/TLS với AWS Certificate Manager
1. Tại trang chủ của AWS [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Gõ ```ACM``` vào thanh tìm kiếm
    + Chọn dịch vụ AWS ACM
    ![CloudFront](/images/6-connect/6.1-acm/001-acm.png?width=90pc)

2. Tại trang chính của AWS Certificate Manager, nhấp chọn **Request a certificate**
    ![CloudFront](/images/6-connect/6.1-acm/002-acm.png?width=90pc)

3. Nhấp chọn **Next** để tạo chứng chỉ SSL/TLS public
    ![CloudFront](/images/6-connect/6.1-acm/003-acm.png?width=90pc)

4. Điền thông tin cần thiết để đăng ký chứng chỉ SSL/TLS cho tên miền **longtruong-lab.online**
    + Fully Qualified domain name: ```longtruong-lab.online```
    + Validation method: **DNS validation**
    + Key algorithm: **RSA 2048**
    + Nhấp chọn **Request**
    ![CloudFront](/images/6-connect/6.1-acm/004-acm.png?width=90pc)

5. Một chứng chỉ sẽ được tạo với trạng thái **Pending validation**
   ![CloudFront](/images/6-connect/6.1-acm/005-acm.png?width=90pc)

6. Trong mục **Domains** của chứng chỉ, chọn **Create records in Route 53**
    ![CloudFront](/images/6-connect/6.1-acm/006-acm.png?width=90pc)

7. Chọn **Create record**
    ![CloudFront](/images/6-connect/6.1-acm/007-acm.png?width=90pc)

8. Đợi khoảng 30 phút để xác thực chứng chỉ. Khi chứng chỉ được xác thực thành công, trạng thái sẽ là <span style="color:green">Issued</span>
    ![CloudFront](/images/6-connect/6.1-acm/008-acm.png?width=90pc)

9. Tại Public Hosted Zone của dịch vụ AWS Route 53, một CNAME record mới sẽ được tạo ra, tương ứng với chứng chỉ đã đăng ký.
    ![CloudFront](/images/6-connect/6.1-acm/009-acm.png?width=90pc)