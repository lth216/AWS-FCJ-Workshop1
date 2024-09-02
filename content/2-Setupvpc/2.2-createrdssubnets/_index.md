---
title : "Create subnet for database server"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

#### Create subnets for database instance
{{% notice note %}}
The Database instance of AWS RDS requires subnets in at least 2 Availability Zones in a given AWS region for high availability. Consequently, we have to create 2 more subnets for database instance.
{{% /notice %}}

1. In the VPC Management Dashboard:
    + Select **Subnets** in the left menu bar.
    + Choose **Create subnet**.

![VPC](/images/2-createvpc/007-createsubnet.png?width=90pc)

{{% notice note %}}
AWS has created 6 subnets (both public and private) with CIDR block /20. According to the allocated subnets, we can see that there are 2 unallocated address ranges: **10.26.32.0 - 10.26.127.255** and **10.26.192.0 - 10.26.255.255**. As a result, we will choose the CIDR block **10.26.192.0/20** and **10.26.208.0/20** for these additional subnets, ensuring that they won't overlap with the existing ones.
{{% /notice %}}

2. Specify the information for the subnet:
    + VPC ID: toggle the list and choose **workshop-1-vpc** VPC.
    + Subnet settings:
        - Subnet name: ```private-subnet-rds-1```.
        - Availability Zone: **us-east-1a**.
        - IPv4 subnet CIDR block: ```10.26.192.0/20```.
    + Click **Create subnet**

![VPC](/images/2-createvpc/008-createsubnet.png?width=40pc)
![VPC](/images/2-createvpc/009-createsubnet.png?width=40pc)

3. Follow step 1 and 2 to create another subnet in Availability Zone **us-east-1b** for database instance. Make sure that you change the subnet information as below 
    + Subnet name: ```private-subnet-rds-2```
    + Availability Zone: **us-east-1b**
    + IPv4 subnet CIDR block: ```10.26.208.0/20``` (increase 16 blocks)

![VPC](/images/2-createvpc/010-createsubnet.png?width=90pc)