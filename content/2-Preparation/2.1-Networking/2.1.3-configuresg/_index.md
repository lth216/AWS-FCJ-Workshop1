---
title : "Create Security Groups"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Create Security Group for Frontend Load Balancer

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.
    
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Frontend-LB-SG```
    + Description: ```Security group for Load Balancer for Frontend``` (Optional)
    + Choose **workshop-1-vpc** VPC
    + Inbound rules: 
      + Type: **HTTP** 
      + Source: **Anywhere IPv4**
    + Click **Create Security group**
    
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/002-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/003-sg.png?width=90pc)

#### Create Security Group for Frontend application

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.
    
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Frontend-SG```
    + Choose **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **Custom TCP**
      + Port: **3000** (Frontend app runs at port 3000)
      + Source: **Frontend-LB-SG** 
    + Click **Create Security group**
    
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/004-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/005-sg.png?width=90pc)

#### Create Security Group for Backend Load Balancer

{{% notice note %}}
Since the backend deploys an API doc using Swagger UI tool to check the connection with the Database server, we will add a rule to allow access from our local IP address in order to interact with the API doc.
{{% /notice %}}

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.
    
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Backend-LB-SG```
    + Choose **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **HTTP**
      + Source: **Anywhere IPv4**
      + Type: **HTTP**
      + Source: **Frontend-SG**
    + Click **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/006-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/007-sg.png?width=90pc)

#### Create Security Group for Backend application

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Backend-SG```
    + Choose **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **Custom TCP**
      + Port: **5214** (Backend application runs at port 5214)
      + Source: **Backend-LB-SG**
    + Click **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/008-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/009-sg.png?width=90pc)

#### Create Security Group for Database instance

1. In the VPC Management Dashboard
    + Select **Security Groups** within the Security section.
    + Choose **Create security group**.

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/001-sg.png?width=90pc)

2. Specify information for Security Group
    + Security group name: ```Database-SG```
    + Choose **workshop-1-vpc** VPC
    + Inbound rules:
      + Type: **MSSQL**
      + Source: **Backend-SG**
    + Click **Create Security group**

    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/010-sg.png?width=90pc)
    ![VPC](/images/2-preparation/2.1-networking/2.3-sg/011-sg.png?width=90pc)