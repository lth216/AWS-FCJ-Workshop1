---
title : "Tạo VPC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Tạo một VPC tùy chọn
1. Đi tới [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
   + Gõ **VPC** vào thanh tìm kiếm.
   + Chọn **VPC**.

![VPC](/images/2-createvpc/001-createvpc.png?width=90pc)

2. Tại trang chủ của VPC:
   + Chọn **Your VPCs** ở bảng menu bên trái.
   + Chọn **Create VPC**.

![VPC](/images/2-createvpc/002-createvpc.png?width=90pc)

3. Khởi tạo VPC với các thông tin sau:
    + Chọn **VPC and more**.
    + Đặt tên cho VPC: ```workshop-1```
    + Tạo IPv4 CIDR block cho VPC: ```10.26.0.0/16```
    + Số lượng Availability Zones: ```2```
    + Số lượng public subnets: ```2```
    + Số lượng private subnets: ```4```
    + Chọn **In 1 AZ** cho NAT Gateway
    + Chọn **None** cho VPC endpoints
    + Giữ mặc định cho các cài đặt khác 
    + Nhấp vào **Create VPC**

![VPC](/images/2-createvpc/003-createvpc.png?width=40pc)
![VPC](/images/2-createvpc/004-createvpc.png?width=40pc)

4. Đợi một vài phút để VPC tiến hành khởi tạo rồi nhấp vào **View VPC** để kiểm tra kết quả

![VPC](/images/2-createvpc/005-createvpc.png?width=40pc)

5. Kiểm tra VPC vừa khởi tạo

![VPC](/images/2-createvpc/006-createvpc.png?width=90pc)