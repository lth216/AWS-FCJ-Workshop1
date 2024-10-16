---
title : "Tạo Security Groups"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Tạo Security Group cho Frontend Load Balancer

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Điền thông tin vào các ô như sau:
    + Security group name: ```Frontend-LB-SG```
    + Description: ```Security group for Load Balancer for Frontend``` (Optional)
    + Chọn **workshop-1-vpc**
    + Inbound rules: 
      + Type: **HTTP** 
      + Source: **Anywhere IPv4**
    + Chọn **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/002-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/003-sg.png?width=90pc)

#### Tạo Security Group cho phần Frontend

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Điền thông tin vào các ô như sau
    + Security group name: ```Frontend-SG```
    + Chọn **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **Custom TCP**
      + Port: **3000** (Frontend chạy ở cổng 3000)
      + Source: **Frontend-LB-SG** 
    + Nhấp chọn **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/004-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/005-sg.png?width=90pc)

#### Tạo Security Group cho Backend Load Balancer

{{% notice note %}}
Bởi vì phần backend có triển khai 1 website để tương tác với API sử dụng Swagger UI tool, chúng ta sẽ thêm 1 rule để cho phép truy cập từ local IP để có thể kiểm tra kết nối giữa backend và cơ sở dữ liệu.
{{% /notice %}}

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Điền thông tin vào các ô như sau
    + Security group name: ```Backend-LB-SG```
    + Chọn **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **HTTP**
      + Source: **My IP**
      + Type: **HTTP**
      + Source: **Frontend-SG**
    + Nhấp chọn **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/006-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/007-sg.png?width=90pc)

#### Tạo Security Group cho phần Backend của ứng dụng

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Điền thông tin vào các ô như sau
    + Security group name: ```Backend-SG```
    + Choose **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **Custom TCP**
      + Port: **5214** (Backend sử dụng cổng 5214)
      + Source: **Backend-LB-SG**
    + Click **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/008-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/009-sg.png?width=90pc)

#### Tạo Security Group cho Database server

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Database-SG```
    + Chọn **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **MSSQL**
      + Source: **Backend-SG**
    + Nhấp chọn **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/010-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/011-sg.png?width=90pc)