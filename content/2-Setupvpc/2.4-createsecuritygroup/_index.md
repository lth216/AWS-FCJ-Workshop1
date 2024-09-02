---
title : "Create Security Groups"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Create Security Groups
Before jumping in the main procedure, we need to have a look at our architecture to analyze the communications.

![Architecture](/images/main_arch.png)

The flow is as below:
- Users accesses to the Frontend of the application from browser using Frontend Load Balancer DNS (**Blue arrows**).
- The firt Load Balancer distributes traffic across Frontend instances in 2 AZs (**Red arrows**)
- Frontend communicates with Backend through a second Load Balancer which distributes traffic across backend instances (**Red arrows**)
- Backend reads and writes data to a AWS RDS primary instance (**Black arrows**)

In the next step, we will create Security Groups for each component to accommodate this workflow.

#### Create Security Group for Frontend Load Balancer

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Frontend-LB-SG```
    + Choose **workshop-1-vpc** VPC
    + Add Inbound rules: **HTTP** for Anywhere IPv4
    + Click **Create Security group**

![VPC](/images/2-createvpc/019-createsg.png?width=90pc)
![VPC](/images/2-createvpc/020-createsg.png?width=90pc)

#### Create Security Group for Frontend application

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Frontend-SG```
    + Choose **workshop-1-vpc** VPC
    + Add Inbound rules:
        - Custom TCP at port 3000 (Frontend app runs at port 3000)
          - Source: **Frontend-LB-SG**
    + Click **Create Security group**

![VPC](/images/2-createvpc/021-createsg.png?width=90pc)
![VPC](/images/2-createvpc/022-createsg.png?width=90pc)

#### Create Security Group for Backend Load Balancer

{{% notice note %}}
Since the backend deploys an API doc using Swagger UI tool, we will add a rule to allow access from our local IP address in order to interact with the API doc and test the connection with Database server.
{{% /notice %}}

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Backend-LB-SG```
    + Choose **workshop-1-vpc** VPC
    + Add Inbound rules:
        - HTTP port 80
          - Source: **My IP**
        - HTTP port 80
          - Source: **Frontend-SG**
    + Click **Create Security group**

![VPC](/images/2-createvpc/023-createsg.png?width=90pc)
![VPC](/images/2-createvpc/024-createsg.png?width=90pc)

#### Create Security Group for Backend application

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Backend-SG```
    + Choose **workshop-1-vpc** VPC
    + Add Inbound rules:
        - Custom TCP at port 5214 (Backend application runs at port 5214)
          - Source: **Backend-LB-SG**
    + Click **Create Security group**

![VPC](/images/2-createvpc/025-createsg.png?width=90pc)
![VPC](/images/2-createvpc/026-createsg.png?width=90pc)

#### Create Security Group for Database instance

{{% notice note %}}
In the previous section, we have configured a route table that allow subnet of Database server to communicate with Internet through Internet Gateway. In this step, we will add rule for the Security Group of Database instance that allows only connections from our local IP address.
After finishing the initialization step of Database server, we can remove this rule  
{{% /notice %}}

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Database-SG```
    + Choose **workshop-1-vpc** VPC
    + Add Inbound rules:
        - MSSQL at port 1433
          - Source: **Backend-SG**
    + Click **Create Security group**

![VPC](/images/2-createvpc/027-createsg.png?width=90pc)
![VPC](/images/2-createvpc/028-createsg-1.png?width=90pc)
![VPC](/images/2-createvpc/028-createsg.png?width=90pc)