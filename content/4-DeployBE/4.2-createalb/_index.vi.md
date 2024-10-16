---
title : "Tạo Load Balancer cho Online Shop Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Tạo Target Groups
1. Tại trang chính của EC2 [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Chọn **Target Groups** trong mục Load Balancing
    + Chọn **Create target group**

    ![BE](/images/4-backend/4.2-alb/001-alb-be.png?width=90pc)

2. Cấu hình Target Group như sau:
    + Target type: **Instance**
        ![BE](/images/4-backend/4.2-alb/002-alb-be.png?width=90pc)
    + Target group name: ```Backend-TG```
    + Protocol: **HTTP port 5214**
    + VPC: **workshop-1-vpc**
        ![BE](/images/4-backend/4.2-alb/003-alb-be.png?width=90pc)
    + Đường dẫn cho Health check: ```/swagger/index.html```
    + Nhấp chọn **Next**
        ![BE](/images/4-backend/4.2-alb/004-alb-be.png?width=90pc)

3. Gán EC2 Instance tương ứng vào Target Group
    + Chọn Online-Shop-Backend và chọn **Include as pending below**.
    + Sau đó, nhấp chọn **Create target group**.

    ![BE](/images/4-backend/4.2-alb/005-alb-be.png?width=80pc)
    ![BE](/images/4-backend/4.2-alb/006-alb-be.png?width=80pc)

4. Kiểm tra kết quả
    ![BE](/images/4-backend/4.2-alb/007-alb-be.png?width=90pc)

#### Tạo Application Load Balancer cho Backend
1. Tại trang chính của [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Chọn **Load Balancers** trong mục Load Balancing.
    + Nhấp chọn **Create load balancer**

    ![BE](/images/4-backend/4.2-alb/008-alb-be.png?width=90pc)

2. Chọn Application Load Balancer

    ![BE](/images/4-backend/4.2-alb/009-alb-be.png?width=90pc)

3. Cấu hình Application Load Balancer cho Backend
{{% notice note %}}
Application Load Balancer nên được đặt ở Public Subnet để ta có thể truy cập vào ứng dụng thông qua tên miền được gán với Load Balancer.
{{% /notice %}}
    + Load balancer name: ```Online-Shop-BE-LB```
    + Scheme: **Internet-facing**
        ![BE](/images/4-backend/4.2-alb/010-alb-be.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Availability Zones: 
        + us-east-1a - Subnet: **workshop-1-subnet-public1-us-east-1a**
        + us-east-1b - Subnet: **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Backend-LB-SG**
        ![BE](/images/4-backend/4.2-alb/011-alb-be.png?width=90pc)
    + Listener: **HTTP** port **80**
    + Target group: **Backend-TG**
        ![BE](/images/4-backend/4.2-alb/012-alb-be.png?width=90pc)
    + Kiểm tra và nhấp chọn **Create load balancer**
        ![BE](/images/4-backend/4.2-alb/013-alb-be.png?width=90pc)

4. Đợi cho tới khi Application Load Balancer khởi tạo
    + Copy tên miền của ALB
    ![BE](/images/4-backend/4.2-alb/014-alb-be.png?width=90pc)


5. Nếu ứng dụng Backend hoạt động bình thường, trạng thái sẽ là <span style="color:green">Healthy</span>
    ![BE](/images/4-backend/4.2-alb/015-alb-be.png?width=90pc)

6. Truy cập vào ứng dụng Backend
    Sử dụng đường dẫn URL: **<_Load_Balancer_DNS_Name>/swagger/index.html**

    ![BE](/images/4-backend/4.2-alb/016-alb-be.png?width=90pc)

7. Kiểm tra kết nối đến Database server
    + Đi đến **GET /api/Product** của bảng Product và chọn **Execute**
    ![BE](/images/4-backend/4.2-alb/017-alb-be.png?width=90pc)
    + Ứng dụng Backend đã kết nối thành công với Database server
