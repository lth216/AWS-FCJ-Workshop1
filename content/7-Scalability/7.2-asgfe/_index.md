---
title : "Create Auto Scaling Group for Frontend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

#### Create custom Amazon Machine Image of Frontend
1. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu bar.
    + Check to the **Online-Shop-Frontend** instance.
    + Toggle **Actions** list and choose **Image and templates**.
    + Choose **Create image**.

    ![FE-ASG](/images/7-test/7.2-asgfe/001-asg-fe.png?width=90pc)

2. Provide information for AMI
    + Image name: ```Online-Shop-FE-AMI```
    + Uncheck reboot instance since we don't want to restart the current Backend EC2 Instance
    + Click **Create image**

    ![FE-ASG](/images/7-test/7.2-asgfe/002-asg-fe.png?width=90pc)

3. Verify result
    ![FE-ASG](/images/7-test/7.2-asgfe/003-asg-fe.png?width=90pc)

#### Create Launch Template for Frontend instance
1. In the EC2 Management Dashboard
    + Choose **Launch Templates**
    + Click **Create launch template**

    ![FE-ASG](/images/7-test/7.2-asgfe/004-asg-fe.png?width=90pc)

2. Specify information for launch template
    + Launch template name: ```Online-Shop-FE-launch-template```
    + Template version description: ```Template cho online shop frontend```
    + Uncheck Auto Scaling guidance
    ![FE-ASG](/images/7-test/7.2-asgfe/005-asg-fe.png?width=90pc)
    + Choose **My AMIs** and **Owned by me**. Then, specify the created AMI in previous step: **Online-Shop-FE-AMI**
    + Instance type: **t2.micro**
    + Key pair: **Don't include in launch template**
    ![FE-ASG](/images/7-test/7.2-asgfe/006-asg-fe.png?width=90pc)
    + Subnet: **Don't include in launch template**
    + Firewall: **Select existing security group**
    + Security groups: **Frontend-SG**
    + Click **Create launch template**
    ![FE-ASG](/images/7-test/7.2-asgfe/007-asg-fe.png?width=90pc)

#### Create Auto Scaling Group
1. In the EC2 Management Dashboard
    + Choose **Auto Scaling Groups** in the left menu
    + Click **Create Auto Scaling group**
    ![FE-ASG](/images/7-test/7.2-asgfe/008-asg-fe.png?width=90pc)

2. Choose instance launch options
    + Auto Scaling group name: ```Online-Shop-FE-ASG```
    + Launch template: **Online-Shop-FE-launch-template**
    + Click **Next**
    
    ![FE-ASG](/images/7-test/7.2-asgfe/009-asg-fe.png?width=90pc)

3. Configure advanced options for Auto Scaling group
    + Network:
      + VPC: **workshop-1-vpc**
      + Availability Zones and subnets:
        + **us-east-1a | workshop-1-subnet-private1-us-east-1a**
        + **us-east-1b | workshop-1-subnet-private2-us-east-1b**
        + Click to **Next**
        ![FE-ASG](/images/7-test/7.2-asgfe/010-asg-fe.png?width=90pc)
    + Load balancing
      + Choose **Attach to an existing load balancer**
      + Choose **Frontend-TG** as Existing load balancer target groups
        ![FE-ASG](/images/7-test/7.2-asgfe/011-asg-fe.png?width=90pc)
    + Health checks
      + Turn on **Elastic Load Balancing health checks** option
      + Click **Next**
        ![FE-ASG](/images/7-test/7.2-asgfe/012-asg-fe.png?width=90pc)
    + Group size and Scaling policy
      + Desired capacity: **1**
      + Min desired capacity: **1**
      + Mas desired capacity: **3**
      + Choose **Target tracking scaling policy**
      + Metric type: **Application Load Balancer request count per target**
      + Target group: **Frontend-TG**
      + Target value: **30**
      ![FE-ASG](/images/7-test/7.2-asgfe/013-asg-fe.png?width=90pc)
    + Instance maintenance policy
      + Uncheck **Enable instance scale-in protection**
      + Click **Next**
      ![FE-ASG](/images/7-test/7.2-asgfe/014-asg-fe.png?width=90pc)
      + Continue to click **Next** until we reach the Review page. Then, click **Create Auto Scaling group**
      ![FE-ASG](/images/7-test/7.2-asgfe/015-asg-fe.png?width=90pc)

4. Attach the current frontend ec2 instance to the Auto Scaling group
    + Choose the **Online-Shop-Frontend**
    + Toggle **Actions** list, choose **Instance settings**
    + Select **Attach to Auto Scaling Group**
    + Select **Online-Shop-FE-ASG** and click **Attach**
    ![FE-ASG](/images/7-test/7.2-asgfe/016-asg-fe.png?width=90pc)
    ![FE-ASG](/images/7-test/7.2-asgfe/017-asg-fe.png?width=90pc)