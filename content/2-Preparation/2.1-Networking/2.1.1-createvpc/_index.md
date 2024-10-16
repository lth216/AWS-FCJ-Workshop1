---
title : "Create VPC"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1.1 </b> "
---

#### Create a custom VPC
1. Navigate to [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
   + Type **VPC** in the search bar.
   + Click **VPC** service.

   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/001-vpc.png?width=90pc)

2. In the VPC Management Dashboard:
   + Select **Your VPCs** in the left menu.
   + Choose **Create VPC**.

   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/002-vpc.png?width=90pc)

3. Provide details for creating VPC:
   + Choose **VPC and more**.
   + Specify name of VPC: ```workshop-1```
   + Specify IPv4 CIDR block for VPC: ```10.26.0.0/16```
   + Number of Availability Zones: **2**
   + Number of public subnets: **2**
   + Number of private subnets: **4**
   + Choose **In 1 AZ** for NAT Gateway.
   + Choose **None** for VPC endpoints.
   + Leave everything else as is.
   + Then click **Create VPC**.

   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/003-vpc.png?width=90pc) 
   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/004-vpc.png?width=90pc)

4. Wait for a few minutes to initialize the VPC and then click **View VPC** to check the result.

   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/005-vpc.png?width=90pc)

5. Verify the created VPC.

   ![VPC](/images/2-preparation/2.1-networking/2.1-vpc/006-vpc.png?width=90pc)