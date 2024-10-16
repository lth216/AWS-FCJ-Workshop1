---
title : "Triển khai dịch vụ AWS CloudFront"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---

#### Tạo CloudFront
1. Tại trang chủ AWS [AWS Management Dashboard](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Gõ ```CloudFront``` vào thanh tìm kiếm
    + Chọn dịch vụ CloudFront
    ![CloudFront](/images/6-connect/6.2-cloudfront/001-cloudfront.png?width=90pc)

2. Tại trang chủ của CloudFront, nhấp chọn **Create CloudFront Distribution**
    ![CloudFront](/images/6-connect/6.2-cloudfront/002-cloudfront.png?width=90pc)

3. Cấu hình CloudFront như sau
    + Origin:
      + Origin domain: **online-shop-fe-lb-681861540.us-east-1.elb.amazonaws.com** (Tên miền của Application Load Balancer cho Frontend)
      + Protocol: **HTTPS only**
      + Để mặc định cho các tùy chọn khác ở mục này
      ![CloudFront](/images/6-connect/6.2-cloudfront/003-cloudfront.png?width=90pc)
    + Default cache behavior
      + Viewer protocol policy: **Redirect HTTP to HTTPS**
      + Allowed HTTP methods: **GET**, **HEAD**
      + Để mặc định cho các tùy chọn khác ở mục này
      ![CloudFront](/images/6-connect/6.2-cloudfront/004-cloudfront.png?width=90pc)
    + Web Application Firewall (WAF)
      + Chọn vào **Do not enable security protections**
      ![CloudFront](/images/6-connect/6.2-cloudfront/005-cloudfront.png?width=90pc)
    + Settings
      + Price class: **Use all edge locations (best performance)**
      + Custom SSL certificate: Chọn chứng chỉ ứng với tên miền **longtruong-lab.online** mà ta đã tạo trước đó
    + Để mặc định cho các tùy chọn khác ở mục này
      ![CloudFront](/images/6-connect/6.2-cloudfront/006-cloudfront.png?width=90pc)
    + Nhấp chọn **Create distribution**
      ![CloudFront](/images/6-connect/6.2-cloudfront/007-cloudfront.png?width=90pc)

4. Tùy thuộc vào số lượng edge location mà ta chọn ở bước trên, thời gian khởi tạo CloudFront có thể sẽ tốn vài phút
    ![CloudFront](/images/6-connect/6.2-cloudfront/008-cloudfront.png?width=90pc)

5. Khi CloudFront hoàn tất việc khởi tạo, ta sẽ thấy hiển t hị thời gian ở mục **Last modified**
    ![CloudFront](/images/6-connect/6.2-cloudfront/009-cloudfront.png?width=90pc)

#### Tạo DNS Record cho dịch vụ CloudFront
1. Tại trang chủ của dịch vụ Route 53, chọn **Create record**
    ![CloudFront](/images/6-connect/6.2-cloudfront/010-cloudfront.png?width=90pc)

2. Cấu hình record như sau
    + Record type: **A**
    + Kích hoạt Alias
    + Route traffic to: **Alias to CloudFront distribution**
    + Region: **us-east-1** (N.Virginia) 
    + Chọn vào tên miền tương ứng với CloudFront distribution
    + Để mặc định cho các tùy chọn khác
    + Nhấp chọn **Create records**
    ![CloudFront](/images/6-connect/6.2-cloudfront/011-cloudfront.png?width=90pc)

3. Kiểm tra kết quả
    ![CloudFront](/images/6-connect/6.2-cloudfront/012-cloudfront.png?width=90pc)

4. Truy cập vào ứng dụng thông qua tên miền đã gán vào CloudFront
    ![CloudFront](/images/6-connect/6.2-cloudfront/013-cloudfront.png?width=90pc)