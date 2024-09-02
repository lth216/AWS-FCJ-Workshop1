---
title : "Create Auto Scaling Group for Backend"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 4.5 </b> "
---

#### Create custom AMI for Backend
1. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu bar.
    + Check to the **Online-Shop-Backend** instance.
    + Toggle **Actions** list and choose **Image and templates**.
    + Choose **Create image**.

![EC2](/images/4-createec2/050-createec2.png?width=90pc)

2. Provide information for AMI
    + Image name: ```Online-Shop-BE-AMI```
    + Uncheck reboot instance
    + Click **Create image**

![EC2](/images/4-createec2/051-createec2.png?width=50pc)

#### Create Launch Template for Backend instance
1. In the EC2 Management Dashboard
    + Choose **Launch Templates**
    + Click **Create launch template**

![EC2](/images/4-createec2/052-createec2.png?width=90pc)

2. Specify information for launch template
    + Launch template name: ```Online-Shop-BE-launch-template```
    + Template version description: ```Template for online shop backend```
    + Uncheck Auto Scaling guidance
    + Choose **My AMIs** and **Owned by me**. Then, specify the created AMI in previous step: **Online-Shop-BE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Online-Shop-BE-SG**
    + Click **Create launch template**

![EC2](/images/4-createec2/053-createec2.png?width=40pc)
![EC2](/images/4-createec2/054-createec2.png?width=40pc)
![EC2](/images/4-createec2/055-createec2.png?width=40pc)
![EC2](/images/4-createec2/056-createec2.png?width=40pc)
![EC2](/images/4-createec2/057-createec2.png?width=40pc)

#### Create Auto Scaling Group
1. In the EC2 Management Dashboard
    + Choose **Auto Scaling Groups** in the left menu
    + Click **Create Auto Scaling group**

![EC2](/images/4-createec2/058-createec2.png?width=90pc)

2. Choose instance launch options
    + Auto Scaling group name: ```Online-Shop-BE-ASG```
    + Launch template: **Online-Shop-BE-launch-template**
    + Click **Next**

![EC2](/images/4-createec2/059-createec2.png?width=40pc)
![EC2](/images/4-createec2/060-createec2.png?width=40pc)

3. Configure advanced options for Auto Scaling Group
    + VPC: **workshop-1-vpc**
    + Availability Zones and subnets:
      + **us-east-1a | workshop-1-subnet-private3-us-east-1a**
      + **us-east-1b | workshop-1-subnet-private4-us-east-1b**
    + Load balancing
      + Choose Attach to an existing load balancer
      + Choose **Backend-TG** as Existing load balancer target groups
      + Turn on **Elastic Load Balancing health checks** option
      + Click **Next**

![EC2](/images/4-createec2/061-createec2.png?width=40pc)
![EC2](/images/4-createec2/062-createec2.png?width=40pc)
![EC2](/images/4-createec2/063-createec2.png?width=40pc)

4. Configure group size and Scaling policy
    + Desired capacity: **1**
    + Min desired capacity: **1**
    + Mas desired capacity: **3**
    + Choose **Target tracking scaling policy**
    + Metric type: **Application Load Balancer request count per target**
    + Target group: **Backend-TG**
    + Target value: **30**
    + Check **Enable instance scale-in protection** to prevent newly launched instance from being terminated.
    + Click **Next**

![EC2](/images/4-createec2/064-createec2.png?width=40pc)
![EC2](/images/4-createec2/065-createec2.png?width=40pc)
![EC2](/images/4-createec2/066-createec2.png?width=40pc)

5. Attach the current backend ec2 instance to the Auto Scaling group
    + Choose the **Online-Shop-Backend**
    + Toggle **Actions** list, choose **Instance settings**
    + Select **Attach to Auto Scaling Group**
    + Select **Online-Shop-BE-ASG** and click **Attach**

![EC2](/images/4-createec2/067-createec2.png?width=90pc)
![EC2](/images/4-createec2/068-createec2.png?width=50pc)