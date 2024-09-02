---
title : "Create Auto Scaling Group for Frontend"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 4.6 </b> "
---

#### Create Auto Scaling Group for Frontend

#### Create custom AMI of Frontend
1. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu bar.
    + Check to the **Online-Shop-Frontend** instance.
    + Toggle **Actions** list and choose **Image and templates**.
    + Choose **Create image**.

![EC2](/images/4-createec2/069-createec2.png?width=90pc)

2. Provide information for AMI
    + Image name: ```Online-Shop-FE-AMI```
    + Uncheck reboot instance
    + Click **Create image**

![EC2](/images/4-createec2/051-createec2.png?width=50pc)

#### Create Launch Template for Frontend instance
1. In the EC2 Management Dashboard
    + Choose **Launch Templates**
    + Click **Create launch template**

![EC2](/images/4-createec2/052-createec2.png?width=90pc)

2. Specify information for launch template
    + Launch template name: ```Online-Shop-FE-launch-template```
    + Template version description: ```Template cho online shop frontend```
    + Uncheck Auto Scaling guidance
    + Choose **My AMIs** and **Owned by me**. Then, specify the created AMI in previous step: **Online-Shop-FE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Online-Shop-FE-SG**
    + Click **Create launch template**

![EC2](/images/4-createec2/070-createec2.png?width=40pc)
![EC2](/images/4-createec2/071-createec2.png?width=40pc)
![EC2](/images/4-createec2/055-createec2.png?width=40pc)
![EC2](/images/4-createec2/072-createec2.png?width=40pc)
![EC2](/images/4-createec2/057-createec2.png?width=40pc)

#### Create Auto Scaling Group
1. In the EC2 Management Dashboard
    + Choose **Auto Scaling Groups** in the left menu
    + Click **Create Auto Scaling group**

![EC2](/images/4-createec2/058-createec2.png?width=90pc)

2. Choose instance launch options
    + Auto Scaling group name: ```Online-Shop-FE-ASG```
    + Launch template: **Online-Shop-FE-launch-template**
    + Click **Next**

![EC2](/images/4-createec2/073-createec2.png?width=40pc)
![EC2](/images/4-createec2/074-createec2.png?width=40pc)

3. Configure advanced options for Auto Scaling group
    + VPC: **workshop-1-vpc**
    + Availability Zones and subnets:
      + **us-east-1a | workshop-1-subnet-private1-us-east-1a**
      + **us-east-1b | workshop-1-subnet-private2-us-east-1b**
    + Load balancing
      + Choose Attach to an existing load balancer
      + Choose **Frontend-TG** as Existing load balancer target groups
      + Turn on **Elastic Load Balancing health checks** option
      + Click **Next**

![EC2](/images/4-createec2/061-createec2.png?width=40pc)
![EC2](/images/4-createec2/075-createec2.png?width=40pc)
![EC2](/images/4-createec2/063-createec2.png?width=40pc)

4. Configure group size and Scaling policy
    + Desired capacity: **1**
    + Min desired capacity: **1**
    + Mas desired capacity: **3**
    + Choose **Target tracking scaling policy**
    + Metric type: **Application Load Balancer request count per target**
    + Target group: **Frontend-TG**
    + Target value: **30**
    + Check **Enable instance scale-in protection** to prevent newly launched instance from being terminated.
    + Click **Next**

![EC2](/images/4-createec2/064-createec2.png?width=40pc)
![EC2](/images/4-createec2/076-createec2.png?width=40pc)
![EC2](/images/4-createec2/066-createec2.png?width=40pc)

5. Attach the current frontend ec2 instance to the Auto Scaling group
    + Choose the **Online-Shop-Frontend**
    + Toggle **Actions** list, choose **Instance settings**
    + Select **Attach to Auto Scaling Group**
    + Select **Online-Shop-FE-ASG** and click **Attach**

![EC2](/images/4-createec2/077-createec2.png?width=90pc)
![EC2](/images/4-createec2/078-createec2.png?width=50pc)