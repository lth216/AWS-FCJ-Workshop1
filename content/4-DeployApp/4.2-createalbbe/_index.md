---
title : "Create Load Balancer for Online Shop Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Create Target Groups
1. In the EC2 Management Dashboard, choose **Target Groups** within the Load Balancing section in the left menu bar and click **Create target group**

![EC2](/images/4-createec2/014-createec2.png?width=90pc)

2. Specify the configuration as follow:
    + Target type: **Instance**
    + Target group name: ```Backend-TG```
    + Protocol: **HTTP port 5214**
    + VPC: **workshop-1-vpc**
    + Health check path: ```/swagger/index.html``` (suffix of API doc website)
    + Click **Next**

![EC2](/images/4-createec2/015-createec2.png?width=40pc)
![EC2](/images/4-createec2/016-createec2.png?width=40pc)
![EC2](/images/4-createec2/017-createec2.png?width=40pc)

3. Attach EC2 Instance to the Target Group
    + Choose Online-Shop-Backend EC2 Instance as Register target and click **Include as pending below**.
    + Then, click **Create target group**

![EC2](/images/4-createec2/018-createec2.png?width=90pc)
![EC2](/images/4-createec2/019-createec2.png?width=90pc)

4. Verify the result

![EC2](/images/4-createec2/020-createec2.png?width=90pc)

#### Create Backend Load Balancer
1. In the EC2 Management Dashboard, choose **Load Balancers** within Load Balancing section and click **Create load balancer**

![EC2](/images/4-createec2/021-createec2.png?width=90pc)

2. Choose Application Load Balancer

![EC2](/images/4-createec2/022-createec2.png?width=40pc)

3. Specify Basic configuration for Backend Load Balancer
    + Load balancer name: ```Online-Shop-BE-LB```
    + Scheme: **Internet-facing**
    + VPC: **workshop-1-vpc**
    + Availability Zones: **workshop-1-subnet-public1-us-east-1a** and **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Backend-LB-SG**
    + Target group: **Backend-TG**
    + Click **Create load balancer**

![EC2](/images/4-createec2/023-createec2.png?width=50pc)
![EC2](/images/4-createec2/024-createec2.png?width=50pc)
![EC2](/images/4-createec2/025-createec2.png?width=50pc)
![EC2](/images/4-createec2/026-createec2.png?width=50pc)

{{% notice note %}}
Although the backend and frontend EC2 instances are deployed within the private subnets, the load balancers should only be deployed onto public subnet so that we can access the website from the browser.
{{% /notice %}}

4. Wait for Backend Load Balancer initialization

![EC2](/images/4-createec2/027-createec2.png?width=90pc)

5. If the Backend application works normally, the status will be <span style="color:green">Healthy</span>

![EC2](/images/4-createec2/028-createec2.png?width=90pc)

6. Access to Load Balancer DNS name from browser

URL: **<_Load_Balancer_DNS_>/swagger/index.html**

![EC2](/images/4-createec2/029-createec2.png?width=90pc)

7. Check the connection to database instance

Toggle the GET /api/Product of Product and choose **Execute**

![EC2](/images/4-createec2/030-createec2.png?width=90pc)
