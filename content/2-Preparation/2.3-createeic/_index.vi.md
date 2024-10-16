---
title : "Tạo EC2 Instance Endpoint Service"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### Tạo EC2 Instance Endpoint Service

{{% notice note %}}
Bởi vì phần Frontend và Backend của ứng dụng được khởi tạo trong các private subnet nên ta không thể truy cập vào các máy ảo này bằng phương pháp SSH truyền thống. Trong trường hợp này, ta sẽ sử dụng EC2 Instance Connect Endpoint để truy cập vào các máy ảo mà không cần phải gán máy ảo với các địa chỉ IP public, từ đó tăng tính bảo mật cho máy chủ.
{{% /notice %}}

#### Tạo Security Group cho EIC endpoint
1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.3-createeic/001-eic.png?width=90pc)

2. Điền thông tin nhu sau
    + Security group name: ```EIC-SG```.
    + Chọn **workshop-1-vpc** VPC.
    + Inbound rules:
      + Type: **SSH**
      + Source: **My IP**
    + Nhấp chọn **Create Security group**

    ![VPC](/images/2-preparation/2.3-createeic/002-eic.png?width=90pc)

#### Tạo EC2 Instance Connect endpoint
1. Tại trang chính của VPC Console
    + Chọn **Endpoints** ở menu bên trái.
    + Chọn **Create endpoint**.

    ![VPC](/images/2-preparation/2.3-createeic/003-eic.png?width=90pc)

2. Điền thông tin cho EIC endpoint
    + Endpoint name tag: ```EIC-Endpoint-Connection```.
    + Service category: **EC2 Instance Connect Endpoint**.
    + VPC: **workshop-1-vpc**.
    + Security groups: ```EIC-SG```
    + Subnet: **workshop-1-subnet-private1-us-east-1a** (Ta có thể chọn giữa subnet 1 and 2)

    ![VPC](/images/2-preparation/2.3-createeic/004-eic.png?width=90pc)
    ![VPC](/images/2-preparation/2.3-createeic/005-eic.png?width=90pc)

3. Đợi vài phút cho tới khi trạng thái của EIC là <span style="color:green">Available</span>

    ![VPC](/images/2-preparation/2.3-createeic/006-eic.png?width=90pc)

#### Thêm inbound rule cho Frontend and Backend Security groups
1. Tại trang chính của VPC
    + Chọn **Security groups** ở menu bên trái
    + Chọn **Frontend-SG** and chọn **Edit inbound rules**

    ![VPC](/images/2-preparation/2.3-createeic/007-eic.png?width=90pc)

2. Thêm Inbound rule
    + Type: **SSH**
    + Source: **EIC Security Group**

    ![VPC](/images/2-preparation/2.3-createeic/008-eic.png?width=90pc)  

4. Tại trang chủ của VPC
    + Chọn **Security groups** ở mục Security
    + Tick vào **Backend-SG** và chọn **Edit inbound rules**

    ![VPC](/images/2-preparation/2.3-createeic/009-eic.png?width=90pc)

5. Thêm Inbound rule
    + Type: **SSH**
    + Source: **EIC Security Group**

    ![VPC](/images/2-preparation/2.3-createeic/010-eic.png?width=90pc)