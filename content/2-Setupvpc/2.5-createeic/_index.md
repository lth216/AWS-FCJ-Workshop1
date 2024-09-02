---
title : "Create EC2 Instance Endpoint Service"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

#### Create EC2 Instance Connect Endpoint

{{% notice note %}}
Since both frontend and backend part of the application are deployed inside private subnets, we cannot login to the instances by traditional SSH method. In this case, we will use an approach called EC2 Instance Connect Endpoint service to access to EC2 instance without needing public IPv4 address.
{{% /notice %}}

#### Create Security Group for EIC endpoint
1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```EIC-SG```.
    + Choose **workshop-1-vpc** VPC.
    + Add Inbound rules:
        - SSH port 22
          - Source: **My IP**.
    + Click **Create Security group**.

![VPC](/images/2-createvpc/029-createeic.png?width=90pc)
![VPC](/images/2-createvpc/030-createeic.png?width=90pc)

#### Create EC2 Instance Connect endpoint
1. In the VPC Management Dashboard
    + Select **Endpoints** in the left menu bar.
    + Select **Create endpoint**.

![VPC](/images/2-createvpc/031-createeic.png?width=90pc)

2. Provide information for the EIC endpoint
    + Endpoint name tag: ```EIC-Endpoint-Connection```.
    + Service category: **EC2 Instance Connect Endpoint**.
    + VPC: **workshop-1-vpc**.
    + Security groups: ```EIC-SG```
    + Subnet: **workshop-1-subnet-private1-us-east-1a** (We can choose randomly between private subnet 1 and 2)

![VPC](/images/2-createvpc/032-createeic.png?width=90pc)
![VPC](/images/2-createvpc/033-createeic.png?width=90pc)

3. Wait for a few minutes until status of EIC endpoint is <span style="color:green">Available</span>

![VPC](/images/2-createvpc/034-createeic.png?width=90pc)

#### Add inbound rule to Frontend and Backend Security groups
1. In the VPC Management Dashboard
    + Choose **Security groups** in the Security section
    + Check the **Frontend-SG** and choose **Edit inbound rules**

![VPC](/images/2-createvpc/035-createeic.png?width=90pc)

2. Add SSH rule from source EIC Security Group

![VPC](/images/2-createvpc/036-createeic.png?width=90pc)

3. Repeat step 1 and 2 for the **Backend-SG** security group. The result is as picture below

![VPC](/images/2-createvpc/037-createeic.png?width=90pc)