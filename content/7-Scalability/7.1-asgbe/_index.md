---
title : "Create Auto Scaling Group for Backend"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

#### Create custom Amazon Machine Image for Backend
1. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu bar.
    + Check to the **Online-Shop-Backend** instance.
    + Toggle **Actions** list and choose **Image and templates**.
    + Choose **Create image**.

    ![BE-ASG](/images/7-test/7.1-asgbe/001-asg-be.png?width=90pc)

2. Provide information for AMI
    + Image name: ```Online-Shop-BE-AMI```
    + Uncheck reboot instance since we don't want to restart the current Backend EC2 Instance
    + Click **Create image**

    ![BE-ASG](/images/7-test/7.1-asgbe/002-asg-be.png?width=90pc)

3. Verify result
    ![BE-ASG](/images/7-test/7.1-asgbe/003-asg-be.png?width=90pc)

#### Create Launch Template for Backend instance
1. In the EC2 Management Dashboard
    + Choose **Launch Templates**
    + Click **Create launch template**

    ![BE-ASG](/images/7-test/7.1-asgbe/004-asg-be.png?width=90pc)

2. Specify information for launch template
    + Launch template name: ```Online-Shop-BE-launch-template```
    + Template version description: ```Template for online shop backend```
    + Uncheck Auto Scaling guidance
    ![BE-ASG](/images/7-test/7.1-asgbe/005-asg-be.png?width=90pc)
    + Choose **My AMIs** and **Owned by me**. Then, choose the created AMI in previous step: **Online-Shop-BE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    + Network settings:
      + Subnet: **Don't include in launch template**
      + Firewall: **Select existing security group**
      + Security groups: **Backend-SG**
    + Click **Create launch template**
    ![BE-ASG](/images/7-test/7.1-asgbe/006-asg-be.png?width=90pc)

#### Create Auto Scaling Group
1. In the EC2 Management Dashboard
    + Choose **Auto Scaling Groups** in the left sidebar
    + Click **Create Auto Scaling group**

    ![BE-ASG](/images/7-test/7.1-asgbe/007-asg-be.png?width=90pc)

2. Choose instance launch options
    + Auto Scaling group name: ```Online-Shop-BE-ASG```
    + Launch template: **Online-Shop-BE-launch-template**
    + Click **Next**

    ![BE-ASG](/images/7-test/7.1-asgbe/008-asg-be.png?width=90pc)

3. Configure advanced options for Auto Scaling Group
    + Network
      + VPC: **workshop-1-vpc**
      + Availability Zones and subnets:
        + **us-east-1a | workshop-1-subnet-private3-us-east-1a**
        + **us-east-1b | workshop-1-subnet-private4-us-east-1b**
    + Click **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/009-asg-be.png?width=90pc)
    + Load Balancing
      + Choose Attach to an existing load balancer
      + Choose **Choose from your load balancer target groups** option
      + Choose **Backend-TG** as Existing load balancer target groups
      
    ![BE-ASG](/images/7-test/7.1-asgbe/010-asg-be.png?width=90pc)
    + Health check
      + Check to **Turn on Elastic Load Balancing health checks** option
    + Click **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/011-asg-be.png?width=90pc)

    + Group size and Scaling policy
      + Desired capacity: **1**
      + Min desired capacity: **1**
      + Mas desired capacity: **3**
      + Choose **Target tracking scaling policy**
      + Metric type: **Application Load Balancer request count per target**
      + Target group: **Backend-TG**
      + Target value: **30**
    + Click **Next**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/012-asg-be.png?width=90pc)
    + Continue to click **Next** until we reach the Review page. Then, click **Create Auto Scaling group**
    
    ![BE-ASG](/images/7-test/7.1-asgbe/013-asg-be.png?width=90pc)

4. Attach the current Backend EC2 Instance to the Auto Scaling group
    + Choose the **Online-Shop-Backend** in the EC2 Dashboard
    + Toggle **Actions** list, choose **Instance settings**
    + Select **Attach to Auto Scaling Group**
    + Select **Online-Shop-BE-ASG** and click **Attach**

    ![BE-ASG](/images/7-test/7.1-asgbe/014-asg-be.png?width=90pc)
    ![BE-ASG](/images/7-test/7.1-asgbe/015-asg-be.png?width=90pc)