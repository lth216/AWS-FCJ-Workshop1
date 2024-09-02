---
title : "Create VPC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Create a custom VPC
1. Navigate to [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
   + Type **VPC** in the search bar.
   + Click **VPC** service.

![VPC](/images/2-createvpc/001-createvpc.png?width=90pc)

2. In the VPC Management Dashboard:
   + Select **Your VPCs** in the left menu.
   + Choose **Create VPC**.

![VPC](/images/2-createvpc/002-createvpc.png?width=90pc)

3. Provide details for creating VPC:
    + Choose **VPC and more**.
    + Specify name of VPC: ```workshop-1```
    + Specify IPv4 CIDR block for VPC: ```10.26.0.0/16```
    + Number of Availability Zones: ```2```
    + Number of public subnets: ```2```
    + Number of private subnets: ```4```
    + Choose **In 1 AZ** for NAT Gateway.
    + Choose **None** for VPC endpoints.
    + Other fields keep default.
    + Then click **Create VPC**.

![VPC](/images/2-createvpc/003-createvpc.png?width=40pc)
![VPC](/images/2-createvpc/004-createvpc.png?width=40pc)

4. Wait for a few minutes and then click **View VPC** to check the created VPC.

![VPC](/images/2-createvpc/005-createvpc.png?width=40pc)

5. Verify the created VPC.

![VPC](/images/2-createvpc/006-createvpc.png?width=90pc)