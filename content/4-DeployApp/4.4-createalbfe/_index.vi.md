---
title : "Tạo Load Balancer cho Online Shop Frontend"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4.4 </b> "
---

#### Create Target Groups
1. Tại trang chính của EC2, chọn **Target Groups** ở trong mục Load Balancing bên trái và nhấp chọn **Create target group**

![EC2](/images/4-createec2/014-createec2.png?width=90pc)

2. Cấu hình Target Group như sau
    + Target type: **Instance**
    + Target group name: ```Frontend-TG```
    + Protocol: **HTTP port 3000**
    + VPC: **workshop-1-vpc**
    + Health check path: **/**
    + Click **Next**

![EC2](/images/4-createec2/015-createec2.png?width=40pc)
![EC2](/images/4-createec2/040-createec2.png?width=40pc)
![EC2](/images/4-createec2/041-createec2.png?width=40pc)

3. Chọn Online-Shop-Frontend EC2 Instance ở Register target và nhấp vào **Include as pending below**. Sau đó, chọn **Create target group**

![EC2](/images/4-createec2/042-createec2.png?width=90pc)
![EC2](/images/4-createec2/043-createec2.png?width=90pc)


#### Tạo Frontend Load Balancer
1. Tại trang chính của EC2, chọn **Load Balancers** trong mục Load Balancing và chọn **Create load balancer**

![EC2](/images/4-createec2/021-createec2.png?width=90pc)

2. Chọn Application Load Balancer

![EC2](/images/4-createec2/022-createec2.png?width=40pc)

3. Cấu hình Frontend Load Balancer
    + Load balancer name: ```Online-Shop-FE-LB```
    + Scheme: **Internet-facing**
    + VPC: **workshop-1-vpc**
    + Availability Zones: **workshop-1-subnet-public1-us-east-1a** và **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Frontend-LB-SG**
    + Target group: **Frontend-TG**
    + Nhấp chọn **Create load balancer**

![EC2](/images/4-createec2/044-createec2.png?width=50pc)
![EC2](/images/4-createec2/024-createec2.png?width=50pc)
![EC2](/images/4-createec2/045-createec2.png?width=50pc)
![EC2](/images/4-createec2/046-createec2.png?width=50pc)

4. Đợi vài phút để khởi tạo Frontend Load Balancer

![EC2](/images/4-createec2/047-createec2.png?width=90pc)

5. Nếu Frontend application hoạt động bình thường, trạng thái sẽ là <span style="color:green">Healthy</span>

![EC2](/images/4-createec2/048-createec2.png?width=90pc)

6. Truy cập vào giao diện web của Online Shop bằng DNS của Load Balancer

URL: **<_Load_Balancer_DNS_>** (Sử dụng DNS của Frontend Load Balancer mà bạn vừa tạo)

![EC2](/images/4-createec2/049-createec2.png?width=90pc)

Ta thấy rằng các Frontend có thể gửi truy vấn API tới Backend để lấy dữ liệu của các mẫu giày.