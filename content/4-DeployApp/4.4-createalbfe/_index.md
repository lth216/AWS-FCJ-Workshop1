---
title : "Create Load Balancer for Online Shop Frontend"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4.4 </b> "
---

#### Create Target Groups
1. In the EC2 Management Dashboard, choose **Target Groups** within the Load Balancing section in the left menu bar and click **Create target group**

![EC2](/images/4-createec2/014-createec2.png?width=90pc)

2. Specify the configuration as follow
    + Target type: **Instance**
    + Target group name: ```Frontend-TG```
    + Protocol: **HTTP port 3000**
    + VPC: **workshop-1-vpc**
    + Health check path: **/**
    + Click **Next**

![EC2](/images/4-createec2/015-createec2.png?width=40pc)
![EC2](/images/4-createec2/040-createec2.png?width=40pc)
![EC2](/images/4-createec2/041-createec2.png?width=40pc)

3. Choose Online-Shop-Frontend EC2 Instance as Register target and click **Include as pending below**. Then, click **Create target group**

![EC2](/images/4-createec2/042-createec2.png?width=90pc)
![EC2](/images/4-createec2/043-createec2.png?width=90pc)


#### Create Frontend Load Balancer
1. In the EC2 Management Dashboard, choose **Load Balancers** within Load Balancing section and click **Create load balancer**

![EC2](/images/4-createec2/021-createec2.png?width=90pc)

2. Choose Application Load Balancer

![EC2](/images/4-createec2/022-createec2.png?width=40pc)

3. Specify Basic configuration for Frontend Load Balancer
    + Load balancer name: ```Online-Shop-FE-LB```
    + Scheme: **Internet-facing**
    + VPC: **workshop-1-vpc**
    + Availability Zones: **workshop-1-subnet-public1-us-east-1a** and **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Frontend-LB-SG**
    + Target group: **Frontend-TG**
    + Click **Create load balancer**

![EC2](/images/4-createec2/044-createec2.png?width=50pc)
![EC2](/images/4-createec2/024-createec2.png?width=50pc)
![EC2](/images/4-createec2/045-createec2.png?width=50pc)
![EC2](/images/4-createec2/046-createec2.png?width=50pc)

4. Wait for Frontend Load Balancer initialization

![EC2](/images/4-createec2/047-createec2.png?width=90pc)

5. If the Frontend application works normally, the status will be <span style="color:green">Healthy</span>

![EC2](/images/4-createec2/048-createec2.png?width=90pc)

6. Access to Load Balancer DNS name from browser

URL: **<_Load_Balancer_DNS_>**

![EC2](/images/4-createec2/049-createec2.png?width=90pc)

We can see that Frontend can communicate with Backend to retrieve data from database server.