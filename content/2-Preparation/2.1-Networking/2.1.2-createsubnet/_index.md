---
title : "Create subnet for database server"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

#### Create subnets for database instance
{{% notice tip %}}
In order to deploy Database instance, the VPC must have at least 2 subnets across different Availability Zones. Additionally, it is recommended to deploy DB instance in separate subnets with the application to restrict the access to the DB instance. Consequently, we will create 2 private subnets in this section for the Database server.\
Reference: [Working with Database instance in a VPC](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#Overview.RDSVPC.Create)
{{% /notice %}}

1. In the VPC Management Dashboard:
    + Select **Subnets** in the left menu bar.
    + Choose **Create subnet**.
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/001-subnet.png?width=90pc)

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
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/002-subnet.png?width=90pc)

3. Create another subnet in the Availability Zone us-east-1b
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/001-subnet.png?width=90pc)

4. Specify the information for the second subnet:
    + VPC ID: toggle the list and choose **workshop-1-vpc** VPC.
    + Subnet settings:
        - Subnet name: ```private-subnet-rds-2```
        - Availability Zone: **us-east-1b**
        - IPv4 subnet CIDR block: ```10.26.208.0/20``` (increase 16 blocks)
    + Click **Create subnet**
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/003-subnet.png?width=90pc)

5. In the end, we have 2 subnets for database instance like this
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/004-subnet.png?width=90pc)