---
title : "Tạo EC2 Instance Endpoint Service"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

#### Tạo EC2 Instance Endpoint Service

{{% notice note %}}
Bởi vì phần Frontend và Backend của ứng dụng được khởi tạo trong các private subnet nên ta không thể truy cập vào các máy ảo này bằng phương pháp SSH truyền thống. Trong trường hợp này, ta sẽ sử dụng EC2 Instance Connect Endpoint để truy cập vào các máy ảo mà không cần phải gán máy ảo với các địa chỉ IP public, từ đó tăng tính bảo mật cho máy chủ.
{{% /notice %}}

#### Tạo Security Group cho EIC endpoint
1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```EIC-SG```.
    + Chọn **workshop-1-vpc** VPC.
    + Thêm Inbound rules:
        - SSH port 22
          - Source: **My IP**
    + Chọn **Create Security group**.

![VPC](/images/2-createvpc/029-createeic.png?width=90pc)
![VPC](/images/2-createvpc/030-createeic.png?width=90pc)

#### Tạo EC2 Instance Connect endpoint
1. Tại trang chính của VPC Console
    + Chọn **Endpoints** ở menu bên trái.
    + Chọn **Create endpoint**.

![VPC](/images/2-createvpc/031-createeic.png?width=90pc)

2. Điền thông tin cho EIC endpoint
    + Endpoint name tag: ```EIC-Endpoint-Connection```.
    + Service category: **EC2 Instance Connect Endpoint**.
    + VPC: **workshop-1-vpc**.
    + Security groups: ```EIC-SG```
    + Subnet: **workshop-1-subnet-private1-us-east-1a** (Ta có thể chọn giữa subnet 1 and 2)

![VPC](/images/2-createvpc/032-createeic.png?width=90pc)
![VPC](/images/2-createvpc/033-createeic.png?width=90pc)

3. Đợi vài phút cho tới khi trạng thái của EIC là <span style="color:green">Available</span>

![VPC](/images/2-createvpc/034-createeic.png?width=90pc)

#### Thêm inbound rule cho Frontend and Backend Security groups
1. Tại trang chính của VPC
    + Chọn **Security groups** ở menu bên trái
    + Chọn **Frontend-SG** and chọn **Edit inbound rules**

![VPC](/images/2-createvpc/035-createeic.png?width=90pc)

2. Thêm SSH rule với Source là EIC Security Group

![VPC](/images/2-createvpc/036-createeic.png?width=90pc)

3. Lặp lại bước 1 và 2 cho **Backend-SG** Security Group. Kết quả sẽ giống như ảnh sau

![VPC](/images/2-createvpc/037-createeic.png?width=90pc)