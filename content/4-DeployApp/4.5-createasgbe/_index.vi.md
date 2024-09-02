---
title : "Tạo Auto Scaling Group cho Backend"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 4.5 </b> "
---

#### Tạo custom AMI cho Backend
1. Tại trang chính của EC2
    + Chọn **Instances** bên thanh menu trái.
    + Tick chọn vào **Online-Shop-Backend** instance.
    + Mở rộng tab **Actions** và chọn **Image và templates**.
    + Chọn **Create image**.

![EC2](/images/4-createec2/050-createec2.png?width=90pc)

2. Điền thông tin của AMI
    + Image name: ```Online-Shop-BE-AMI```
    + Bỏ chọn reboot instance
    + Nhấp vào **Create image**

![EC2](/images/4-createec2/051-createec2.png?width=50pc)

#### Tạo Launch Template cho Backend instance
1. Tại trang chính của EC2
    + Chọn **Launch Templates**
    + Nhấp chọn **Create launch template**

![EC2](/images/4-createec2/052-createec2.png?width=90pc)

2. Cấu hình launch template
    + Launch template name: ```Online-Shop-BE-launch-template```
    + Template version description: ```Template for online shop backend```
    + Bỏ chọn Auto Scaling guidance
    + Chọn **My AMIs** và **Owned by me**. Sau đó, chọn AMI ở bước trước: **Online-Shop-BE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Online-Shop-BE-SG**
    + Nhấp chọn **Create launch template**

![EC2](/images/4-createec2/053-createec2.png?width=40pc)
![EC2](/images/4-createec2/054-createec2.png?width=40pc)
![EC2](/images/4-createec2/055-createec2.png?width=40pc)
![EC2](/images/4-createec2/056-createec2.png?width=40pc)
![EC2](/images/4-createec2/057-createec2.png?width=40pc)

#### Tạo Auto Scaling Group
1. Tại trang chính của EC2
    + Chọn **Auto Scaling Groups** ở thanh menu trái
    + Nhấp chọn **Create Auto Scaling group**

![EC2](/images/4-createec2/058-createec2.png?width=90pc)

2. Chọn instance launch options
    + Auto Scaling group name: ```Online-Shop-BE-ASG```
    + Launch template: **Online-Shop-BE-launch-template**
    + Nhấp chọn **Next**

![EC2](/images/4-createec2/059-createec2.png?width=40pc)
![EC2](/images/4-createec2/060-createec2.png?width=40pc)

3. Cấu hình tùy chọn nâng cao cho Auto Scaling Group
    + VPC: **workshop-1-vpc**
    + Availability Zones và subnets:
      + **us-east-1a | workshop-1-subnet-private3-us-east-1a**
      + **us-east-1b | workshop-1-subnet-private4-us-east-1b**
    + Load balancing
      + Chọn **Attach to an existing load balancer**
      + Chọn **Backend-TG** as Existing load balancer target groups
      + Bật **Elastic Load Balancing health checks** chức năng
      + Nhấp chọn **Next**

![EC2](/images/4-createec2/061-createec2.png?width=40pc)
![EC2](/images/4-createec2/062-createec2.png?width=40pc)
![EC2](/images/4-createec2/063-createec2.png?width=40pc)

4. Cấu hình group size và Scaling policy
    + Desired capacity: **1**
    + Min desired capacity: **1**
    + Mas desired capacity: **3**
    + Chọn **Target tracking scaling policy**
    + Metric type: **Application Load Balancer request count per target**
    + Target group: **Backend-TG**
    + Target value: **30**
    + Chọn **Enable instance scale-in protection** để không xóa các instance được thêm vào bởi ASG.
    + Nhấp chọn **Next**

![EC2](/images/4-createec2/064-createec2.png?width=40pc)
![EC2](/images/4-createec2/065-createec2.png?width=40pc)
![EC2](/images/4-createec2/066-createec2.png?width=40pc)

5. Gán Backend EC2 instance hiện tại vào Auto Scaling Group
    + Chọn the **Online-Shop-Backend**
    + Mở rộng **Actions**, chọn **Instance settings**
    + Chọn **Attach to Auto Scaling Group**
    + Chọn **Online-Shop-BE-ASG** và nhấp chọn **Attach**

![EC2](/images/4-createec2/067-createec2.png?width=90pc)
![EC2](/images/4-createec2/068-createec2.png?width=50pc)