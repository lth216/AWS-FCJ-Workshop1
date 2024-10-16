---
title : "Create EC2 Instance Endpoint Service"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### Create EC2 Instance Connect Endpoint

{{% notice note %}}
Since both frontend and backend part of the application are deployed inside private subnets, we cannot login to the instances by traditional SSH method. In this case, we will use an approach called EC2 Instance Connect Endpoint service to access to EC2 instance without needing public IPv4 address.
{{% /notice %}}

#### Create Security Group for EIC endpoint
1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.
        ![VPC](/images/2-preparation/2.3-createeic/001-eic.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```EIC-SG```.
    + Choose **workshop-1-vpc** VPC.
    + Inbound rules:
      + Type: **SSH**
      + Source: **My IP**
    + Click **Create Security group**

    ![VPC](/images/2-preparation/2.3-createeic/002-eic.png?width=90pc)
    
#### Create EC2 Instance Connect endpoint
1. In the VPC Management Dashboard
    + Select **Endpoints** in the left menu bar.
    + Select **Create endpoint**.

    ![VPC](/images/2-preparation/2.3-createeic/003-eic.png?width=90pc)

2. Provide information for the EIC endpoint
    + Endpoint name tag: ```EIC-Endpoint-Connection```.
    + Service category: **EC2 Instance Connect Endpoint**.
    + VPC: **workshop-1-vpc**.
    + Security groups: ```EIC-SG```
    + Subnet: **workshop-1-subnet-private1-us-east-1a** (We can choose randomly between private subnet 1 and 2)

    ![VPC](/images/2-preparation/2.3-createeic/004-eic.png?width=90pc)
    ![VPC](/images/2-preparation/2.3-createeic/005-eic.png?width=90pc)

3. Wait for a few minutes until status of EIC endpoint is <span style="color:green">Available</span>

    ![VPC](/images/2-preparation/2.3-createeic/006-eic.png?width=90pc)

#### Add inbound rule to Frontend and Backend Security groups
1. In the VPC Management Dashboard
    + Choose **Security groups** in the Security section
    + Check the **Frontend-SG** and choose **Edit inbound rules**

    ![VPC](/images/2-preparation/2.3-createeic/007-eic.png?width=90pc)

2. Add Inbound rule
    + Type: **SSH**
    + Source: **EIC Security Group**

    ![VPC](/images/2-preparation/2.3-createeic/008-eic.png?width=90pc)

4. In the VPC Management Dashboard
    + Choose **Security groups** in the Security section
    + Check the **Backend-SG** and choose **Edit inbound rules**
    ![VPC](/images/2-preparation/2.3-createeic/009-eic.png?width=90pc)

5. Add Inbound rule
    + Type: **SSH**
    + Source: **EIC Security Group**

    ![VPC](/images/2-preparation/2.3-createeic/010-eic.png?width=90pc)