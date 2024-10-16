---
title : "Deploy Database server"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Create AWS RDS
{{% notice note %}}
When it comes to database deployment, it is suggested to have one or more standby DB instances across different Availability Zones, mitigating the workloads on the primary DB instance and also providing high availability and failover support for our database system. However, in this workshop, we just deploy one Primary DB instance with Single-AZ deployment to reduce the cost.\
Reference: [AWS MSSQL Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)
{{% /notice %}}

#### Create Subnet Group
{{% notice info %}}
Amazon RDS will choose a subnet and an IP address from this Subnet Group to associate with your DB instance. The DB instance will be deployed in the Availability Zone that contains the subnet, and managed by AWS.
{{% /notice %}}

1. Type **RDS** in the search bar of AWS Management Console and choose AWS RDS service

   ![RDS](/images/3-database/001-rds.png?width=90pc)

2. In the AWS RDS dashboard, choose Subnet groups and click **Create DB subnet group**

   ![RDS](/images/3-database/002-rds.png?width=90pc)

3. Specify details for Subnet group
    + Name: ```workshop-1-db-subnet-group```
    + Description: ```Subnet group for database instance```
    + VPC: choose **workshop-1-vpc**
      ![RDS](/images/3-database/003-rds.png?width=90pc)
    + Availability Zones: choose **us-east-1a** and **us-east-1b**
    + Subnets: choose **private-subnet-rds-1** (10.26.208.0/20) and **private-subnet-rds-2** (10.26.192.0/20)
    + Click **Create**
      ![RDS](/images/3-database/004-rds.png?width=90pc)

4. Verify the result

   ![RDS](/images/3-database/005-rds.png?width=90pc)

#### Create Database instance

1. Choose **Databases** in the left sidebar bar and choose **Create database**

   ![RDS](/images/3-database/006-rds.png?width=90pc)

2. Provide details for Database instance
   + Database creation method: **Standard create**
   + Engine options: **Microsoft SQL Server**
      ![RDS](/images/3-database/007-rds.png?width=90pc)
   + Database management type: **Amazon RDS**
   + Edition: **SQL Server Express Edition** (support Free tier)
   + Templates: **Free tier**
      ![RDS](/images/3-database/008-rds.png?width=90pc)
   + DB instance identifier: ```online-shop-database```
   + Master username: ```admin```
   + Credentials management: **Self managed**
   + Master password: (Specify your password)
   + Confirm master password: (Specify your password)
      ![RDS](/images/3-database/009-rds.png?width=90pc)
   + VPC: **workshop-1-vpc**
   + DB subnet group: **workshop-1-db-subnet-group**
   + Public access: **No**
      ![RDS](/images/3-database/010-rds.png?width=90pc)
   + Existing VPC security groups: **Database-SG**
   + Availability Zone: **us-east-1a** (or **us-east-1b**)
      ![RDS](/images/3-database/011-rds.png?width=90pc)
   + Turn off Performance Insights in Monitoring section
      ![RDS](/images/3-database/012-rds.png?width=90pc)
   + Click **Create database** after reviewing all the information
      ![RDS](/images/3-database/013-rds.png?width=90pc)

3. Wait for a few minutes to initilize the database instance
   + Save the endpoint of Database instance for later usage.
   ![RDS](/images/3-database/014-rds.png?width=90pc)