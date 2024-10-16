---
title : "Tạo Load Balancer cho Online Shop Frontend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

#### Tạo Target Groups
1. Tại trang chính của EC2 [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Chọn **Target Groups** trong mục Load Balancing
    + Chọn **Create target group**
    
    ![FE](/images/5-frontend/5.2-alb/001-alb-fe.png?width=90pc)

2. Cấu hình Target Group như sau:
    + Target type: **Instance**
    + Target group name: ```Frontend-TG```
    + Protocol: **HTTP port 3000**
    + IP address type: IPv4
        ![FE](/images/5-frontend/5.2-alb/002-alb-fe.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Đường dẫn cho Health check: **/**
    + Nhấp chọn **Next**
        ![FE](/images/5-frontend/5.2-alb/003-alb-fe.png?width=90pc)

3. Gán EC2 Instance tương ứng vào Target Group
    + Chọn Online-Shop-Frontend EC2 Instance và chọn **Include as pending below**.
    + Sau đó, nhấp chọn **Create target group**
    ![FE](/images/5-frontend/5.2-alb/004-alb-fe.png?width=90pc)

4. Kiểm tra kết quả
    ![FE](/images/5-frontend/5.2-alb/005-alb-fe.png?width=90pc)


#### Tạo Application Load Balancer cho Frontend
1. Tại trang chính của [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Chọn **Load Balancers** trong mục Load Balancing.
    + Nhấp chọn **Create load balancer**
    ![FE](/images/5-frontend/5.2-alb/006-alb-fe.png?width=90pc)

2. Chọn Application Load Balancer
    ![FE](/images/5-frontend/5.2-alb/007-alb-fe.png?width=90pc)

3. Cấu hình Application Load Balancer cho Backend
{{% notice note %}}
Application Load Balancer nên được đặt ở Public Subnet để ta có thể truy cập vào ứng dụng thông qua tên miền được gán với Load Balancer.
{{% /notice %}}
    + Load balancer name: ```Online-Shop-FE-LB```
    + Scheme: **Internet-facing**
        ![FE](/images/5-frontend/5.2-alb/008-alb-fe.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Availability Zones:
        + us-east-1a - Subnet: **workshop-1-subnet-public1-us-east-1a**
        + us-east-1b - Subnet: **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Frontend-LB-SG**
        ![FE](/images/5-frontend/5.2-alb/009-alb-fe.png?width=90pc)
    + Listener: **HTTP** port **80**
    + Target group: **Frontend-TG**
        ![FE](/images/5-frontend/5.2-alb/010-alb-fe.png?width=90pc)
    + Kiểm tra và nhấp chọn **Create load balancer**
        ![FE](/images/5-frontend/5.2-alb/011-alb-fe.png?width=90pc)

4. Nếu ứng dụng Frontend hoạt động bình thường, trạng thái sẽ là <span style="color:green">Healthy</span>
    ![FE](/images/5-frontend/5.2-alb/012-alb-fe.png?width=90pc)

5. Truy cập vào Frontend
    Sử dụng tên miền của ALB: **<_Load_Balancer_DNS_>**
    ![FE](/images/5-frontend/5.2-alb/013-alb-fe.png?width=90pc)

Ta có thể thấy rằng Frontend đã kết nối thành công với Backend.