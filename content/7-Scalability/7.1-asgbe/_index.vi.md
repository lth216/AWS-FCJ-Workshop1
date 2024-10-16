---
title : "Tạo Auto Scaling Group cho Backend"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

#### Tạo custom AMI cho Backend
1. Tại trang chính của EC2
    + Chọn **Instances** bên thanh menu trái.
    + Tick chọn vào **Online-Shop-Backend** instance.
    + Mở rộng tab **Actions** và chọn **Image và templates**.
    + Chọn **Create image**.

    ![BE-ASG](/images/7-test/7.1-asgbe/001-asg-be.png?width=90pc)

2. Điền thông tin của AMI
    + Image name: ```Online-Shop-BE-AMI```
    + Bỏ chọn reboot instance
    + Nhấp vào **Create image**

    ![BE-ASG](/images/7-test/7.1-asgbe/002-asg-be.png?width=90pc)

3. Kiểm tra kết quả
    ![BE-ASG](/images/7-test/7.1-asgbe/003-asg-be.png?width=90pc)

#### Tạo Launch Template cho Backend instance
1. Tại trang chính của EC2
    + Chọn **Launch Templates**
    + Nhấp chọn **Create launch template**

    ![BE-ASG](/images/7-test/7.1-asgbe/004-asg-be.png?width=90pc)

2. Cấu hình launch template
    + Launch template name: ```Online-Shop-BE-launch-template```
    + Template version description: ```Template for online shop backend```
    + Bỏ chọn Auto Scaling guidance
    ![BE-ASG](/images/7-test/7.1-asgbe/005-asg-be.png?width=90pc)
    + Chọn **My AMIs** và **Owned by me**. Sau đó, chọn AMI ở bước trước: **Online-Shop-BE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Online-Shop-BE-SG**
    + Nhấp chọn **Create launch template**
    ![BE-ASG](/images/7-test/7.1-asgbe/006-asg-be.png?width=90pc)

#### Tạo Auto Scaling Group
1. Tại trang chính của EC2
    + Chọn **Auto Scaling Groups** ở thanh menu trái
    + Nhấp chọn **Create Auto Scaling group**

    ![BE-ASG](/images/7-test/7.1-asgbe/007-asg-be.png?width=90pc)

2. Chọn instance launch options
    + Auto Scaling group name: ```Online-Shop-BE-ASG```
    + Launch template: **Online-Shop-BE-launch-template**
    + Nhấp chọn **Next**

    ![BE-ASG](/images/7-test/7.1-asgbe/008-asg-be.png?width=90pc)

3. Cấu hình tùy chọn nâng cao cho Auto Scaling Group
   + Ở mục Network:
      + VPC: **workshop-1-vpc**
      + Availability Zones and subnets:
        + **us-east-1a | workshop-1-subnet-private3-us-east-1a**
        + **us-east-1b | workshop-1-subnet-private4-us-east-1b**
    + Nhấp chọn **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/009-asg-be.png?width=90pc)
    + Load Balancing
      + Chọn **Attach to an existing load balancer**
      + Chọn **Choose from your load balancer target groups**
      + Chọn **Backend-TG**
      
    ![BE-ASG](/images/7-test/7.1-asgbe/010-asg-be.png?width=90pc)
    + Health check
      + Chọn **Turn on Elastic Load Balancing health checks**
    + Nhấp chọn **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/011-asg-be.png?width=90pc)

    + Ở mục Group size and Scaling policy
      + Desired capacity: **1**
      + Min desired capacity: **1**
      + Mas desired capacity: **3**
      + Chọn **Target tracking scaling policy**
      + Metric type: **Application Load Balancer request count per target**
      + Target group: **Backend-TG**
      + Target value: **30**
    + Nhấp chọn **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/012-asg-be.png?width=90pc)
    + Tiếp tục chọn **Next** cho đến trang Review. Sau đó, chọn **Create Auto Scaling group**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/013-asg-be.png?width=90pc)

4. Gán EC2 Instance đang chạy phần Backend ở hiện tại vào Auto-scaling group để tiến hành quản lí chung
    + Chọn **Online-Shop-Backend** ở trang dịch vụ của EC2
    + Mở **Actions**, chọn **Instance settings**
    + Chọn **Attach to Auto Scaling Group**
    + Chọn **Online-Shop-BE-ASG** và nhấp chọn **Attach**

    ![BE-ASG](/images/7-test/7.1-asgbe/014-asg-be.png?width=90pc)
    ![BE-ASG](/images/7-test/7.1-asgbe/015-asg-be.png?width=90pc)