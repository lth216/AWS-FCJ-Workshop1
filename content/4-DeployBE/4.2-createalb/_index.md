---
title : "Create Load Balancer for Online Shop Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Create Target Groups
1. In the [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Choose **Target Groups** within the Load Balancing section in the left menu bar
    + Click **Create target group**

    ![BE](/images/4-backend/4.2-alb/001-alb-be.png?width=90pc)

2. Specify the configuration as follow:
    + Target type: **Instance**
        ![BE](/images/4-backend/4.2-alb/002-alb-be.png?width=90pc)
    + Target group name: ```Backend-TG```
    + Protocol: **HTTP port 5214**
    + VPC: **workshop-1-vpc**
        ![BE](/images/4-backend/4.2-alb/003-alb-be.png?width=90pc)
    + Health check path: ```/swagger/index.html```
    + Click **Next**
        ![BE](/images/4-backend/4.2-alb/004-alb-be.png?width=90pc)

3. Attach EC2 Instance to the Target Group
    + Choose Online-Shop-Backend EC2 Instance click **Include as pending below**.
    + Then, click **Create target group**.

    ![BE](/images/4-backend/4.2-alb/005-alb-be.png?width=80pc)
    ![BE](/images/4-backend/4.2-alb/006-alb-be.png?width=80pc)

4. Verify the result

    ![BE](/images/4-backend/4.2-alb/007-alb-be.png?width=90pc)

#### Create Backend Load Balancer
1. In the [EC2 Management Dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:)
    + Choose **Load Balancers** within Load Balancing section.
    + Click **Create load balancer**

    ![BE](/images/4-backend/4.2-alb/008-alb-be.png?width=90pc)

2. Choose Application Load Balancer

    ![BE](/images/4-backend/4.2-alb/009-alb-be.png?width=90pc)

3. Specify Basic configuration for Backend Load Balancer
{{% notice note %}}
The load balancers should be deployed in public subnet so that we can access the application which is deployed in private subnets from web browser.
{{% /notice %}}
    + Load balancer name: ```Online-Shop-BE-LB```
    + Scheme: **Internet-facing**
        ![BE](/images/4-backend/4.2-alb/010-alb-be.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Availability Zones: 
        + us-east-1a - Subnet: **workshop-1-subnet-public1-us-east-1a**
        + us-east-1b - Subnet: **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Backend-LB-SG**
        ![BE](/images/4-backend/4.2-alb/011-alb-be.png?width=90pc)
    + Listener: **HTTP** port **80**
    + Target group: **Backend-TG**
        ![BE](/images/4-backend/4.2-alb/012-alb-be.png?width=90pc)
    + Review and click **Create load balancer**
        ![BE](/images/4-backend/4.2-alb/013-alb-be.png?width=90pc)

4. Wait for Backend Load Balancer initialization
    + Copy the Load Balancer DNS Name
    ![BE](/images/4-backend/4.2-alb/014-alb-be.png?width=90pc)


5. If the Backend application works normally, the status will be <span style="color:green">Healthy</span>
    ![BE](/images/4-backend/4.2-alb/015-alb-be.png?width=90pc)

6. Access to Backend application
    Using this URL: **<_Load_Balancer_DNS_Name>/swagger/index.html**

    ![BE](/images/4-backend/4.2-alb/016-alb-be.png?width=90pc)

7. Check the connection to database instance
    + Toggle the **GET /api/Product** of Product table and choose **Execute**
    ![BE](/images/4-backend/4.2-alb/017-alb-be.png?width=90pc)
    + The Backend application successfully connects to the Database server.