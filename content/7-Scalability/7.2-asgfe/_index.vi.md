---
title : "Tạo Auto Scaling Group cho Frontend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

#### Tạo Amazon Image tùy chỉnh cho Frontend
1. Tại trang chính của EC2
    + Chọn **Instances** bên thanh menu trái.
    + Tick chọn vào **Online-Shop-Frontend** instance.
    + Mở rộng tab **Actions** và chọn **Image và templates**.
    + Chọn **Create image**.

    ![FE-ASG](/images/7-test/7.2-asgfe/001-asg-fe.png?width=90pc)

2. Điền thông tin của AMI
    + Image name: ```Online-Shop-FE-AMI```
    + Bỏ chọn reboot instance vì ta không muốn khởi động lại EC2 Instance dùng cho Frontend đang chạy
    + Nhấp vào **Create image**

    ![FE-ASG](/images/7-test/7.2-asgfe/002-asg-fe.png?width=90pc)

3. Kiểm tra kết quả
    ![FE-ASG](/images/7-test/7.2-asgfe/003-asg-fe.png?width=90pc)

#### Tạo Launch Template cho Frontend instance
1. Tại trang chính của EC2
    + Chọn **Launch Templates**
    + Nhấp chọn **Create launch template**

    ![FE-ASG](/images/7-test/7.2-asgfe/004-asg-fe.png?width=90pc)

2. Cấu hình launch template
    + Launch template name: ```Online-Shop-FE-launch-template```
    + Template version description: ```Template cho online shop frontend```
    + Bỏ chọn Auto Scaling guidance
    ![FE-ASG](/images/7-test/7.2-asgfe/005-asg-fe.png?width=90pc)
    + Chọn **My AMIs** và **Owned by me**. Sau đó, chọn AMI ở bước trước: **Online-Shop-FE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    ![FE-ASG](/images/7-test/7.2-asgfe/006-asg-fe.png?width=90pc)
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Online-Shop-FE-SG**
    + Nhấp chọn **Create launch template**
    ![FE-ASG](/images/7-test/7.2-asgfe/007-asg-fe.png?width=90pc)

#### Tạo Auto Scaling Group
1. Tại trang chính của EC2
    + Chọn **Auto Scaling Groups** ở thanh menu trái
    + Nhấp chọn **Create Auto Scaling group**
    ![FE-ASG](/images/7-test/7.2-asgfe/008-asg-fe.png?width=90pc)

2. Chọn instance launch options
    + Auto Scaling group name: ```Online-Shop-FE-ASG```
    + Launch template: **Online-Shop-BE-launch-template**
    + Nhấp chọn **Next**

    ![FE-ASG](/images/7-test/7.2-asgfe/009-asg-fe.png?width=90pc)

3. Cấu hình cho Auto Scaling group
    + Ở mục VPC: **workshop-1-vpc**
    + Ở mục Availability Zones và subnets:
      + **us-east-1a | workshop-1-subnet-private1-us-east-1a**
      + **us-east-1b | workshop-1-subnet-private2-us-east-1b**
    + Ở mục Load balancing
      + Chọn **Attach to an existing load balancer**
      + Chọn **Frontend-TG**
      + Bật **Elastic Load Balancing health checks** chức năng
      + Nhấp chọn **Next**
      ![FE-ASG](/images/7-test/7.2-asgfe/010-asg-fe.png?width=90pc)
    + Ở mục Health checks
      + Bật **Elastic Load Balancing health checks** 
      + Nhấp chọn **Next**
        ![FE-ASG](/images/7-test/7.2-asgfe/012-asg-fe.png?width=90pc)
    + Ở mục Group size and Scaling policy
      + Desired capacity: **1**
      + Min desired capacity: **1**
      + Mas desired capacity: **3**
      + Chọn **Target tracking scaling policy**
      + Metric type: **Application Load Balancer request count per target**
      + Target group: **Frontend-TG**
      + Target value: **30**
      ![FE-ASG](/images/7-test/7.2-asgfe/013-asg-fe.png?width=90pc)
    + Ở mục Instance maintenance policy
      + Bỏ chọn **Enable instance scale-in protection**
      + Nhấp chọn **Next**
      ![FE-ASG](/images/7-test/7.2-asgfe/014-asg-fe.png?width=90pc)
      + Tiếp tục chọn **Next** tới khi đến trang Review. Sau đó, nhấn chọn **Create Auto Scaling group**
      ![FE-ASG](/images/7-test/7.2-asgfe/015-asg-fe.png?width=90pc)

4. Gán EC2 Instance cho Frontend hiện tại vào Auto-scaling group
    + Chọn **Online-Shop-Frontend** ở trang chủ EC2
    + Mở **Actions** và chọn **Instance settings**
    + Chọn **Attach to Auto Scaling Group**
    + Chọn **Online-Shop-FE-ASG** và nhấp chọn **Attach**
    ![FE-ASG](/images/7-test/7.2-asgfe/016-asg-fe.png?width=90pc)
    ![FE-ASG](/images/7-test/7.2-asgfe/017-asg-fe.png?width=90pc)