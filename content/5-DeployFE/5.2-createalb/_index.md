---
title : "Create Load Balancer for Online Shop Frontend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

#### Create Target Groups
1. In the [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Choose **Target Groups** within the Load Balancing section in the left menu bar
    + Click **Create target group**

    ![FE](/images/5-frontend/5.2-alb/001-alb-fe.png?width=90pc)

2. Specify the configuration as follow
    + Target type: **Instance**
    + Target group name: ```Frontend-TG```
    + Protocol: **HTTP port 3000**
    + IP address type: IPv4
        ![FE](/images/5-frontend/5.2-alb/002-alb-fe.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Health check path: **/**
    + Click **Next**
        ![FE](/images/5-frontend/5.2-alb/003-alb-fe.png?width=90pc)

3. Attach EC2 Instance to the Target Group
    + Choose Online-Shop-Frontend EC2 Instance as Register target and click **Include as pending below**.
    + Then, click **Create target group**
    ![FE](/images/5-frontend/5.2-alb/004-alb-fe.png?width=90pc)

4. Verify the result
    ![FE](/images/5-frontend/5.2-alb/005-alb-fe.png?width=90pc)

#### Create Frontend Load Balancer
1. In the [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Choose **Load Balancers** within Load Balancing section.
    + Click **Create load balancer**
    ![FE](/images/5-frontend/5.2-alb/006-alb-fe.png?width=90pc)

2. Choose Application Load Balancer
    ![FE](/images/5-frontend/5.2-alb/007-alb-fe.png?width=90pc)

3. Specify Basic configuration for Backend Load Balancer
{{% notice note %}}
The load balancers should be deployed in public subnet so that we can access the application which is deployed in private subnets from web browser.
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
    + Review and click **Create load balancer**
        ![FE](/images/5-frontend/5.2-alb/011-alb-fe.png?width=90pc)

4. If the Frontend application works normally, the status will be <span style="color:green">Healthy</span>
    ![FE](/images/5-frontend/5.2-alb/012-alb-fe.png?width=90pc)

5. Access to Frontend application
    Using Load Balancer domain: **<_Load_Balancer_DNS_>**
    ![FE](/images/5-frontend/5.2-alb/013-alb-fe.png?width=90pc)

We can see that Frontend can communicate with Backend to retrieve data from database server.