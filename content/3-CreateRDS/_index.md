---
title : "Create Database server with AWS RDS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Create AWS RDS
{{% notice note %}}
In order to reduce cost of this workshop, we only deploy 1 MSSQL database server on Single-AZ in this workshop. However, it is recommended to deploy at least 1 Primary and 1 Standby Database instance in Multi-AZ to reduce the workload on 1 instance, ensuring the high availability of the Database server.
{{% /notice %}}

For the pricing of MSSQL AWS RDS, refer here: [AWS MSSQL Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)

#### Create Subnet Group
{{% notice info %}}
Amazon RDS chooses a subnet and an IP address within the subnet group that you created to associate with your DB instance. The DB instance uses the Availability Zone that contains the subnet.
{{% /notice %}}

1. Type **RDS** in the search bar of AWS Management Console and choose AWS RDS service

![RDS](/images/3-createrds/001-createrds.png?width=50pc)

2. In the AWS RDS dashboard, choose Subnet groups and click **Create DB subnet group**

![RDS](/images/3-createrds/002-createrds-1.png?width=90pc)

3. Specify details for Subnet group
    + Name: ```workshop-1-db-subnet-group```
    + Description: ```Subnet group for database instance```
    + VPC: **workshop-1-vpc**
    + Availability Zones: **us-east-1a** and **us-east-1b**
    + Subnets: **private-subnet-rds-1** and **private-subnet-rds-2**
    + Click **Create**

![RDS](/images/3-createrds/003-createrds.png?width=40pc)
![RDS](/images/3-createrds/004-createrds.png?width=40pc)

4. Verify the result

![RDS](/images/3-createrds/005-createrds.png?width=90pc)

#### Create Database instance

1. Choose **Databases** in the left menu bar and choose **Create database**

![VPC](/images/3-createrds/006-createrds-1.png?width=90pc)

2. Provide details for Database instance
   + Database creation method: **Standard create**
   + Engine options: **Microsoft SQL Server**
   + Database management type: **Amazon RDS**
   + Edition: **SQL Server Express Edition** (support Free tier)
   + Templates: **Free tier**

![RDS](/images/3-createrds/011-createrds.png?width=40pc)
![RDS](/images/3-createrds/012-createrds.png?width=40pc)
![RDS](/images/3-createrds/013-createrds.png?width=40pc)
![RDS](/images/3-createrds/014-createrds.png?width=40pc)

   + Settings:
     + DB instance identifier: **online-shop-database**
     + Master username: **admin**
     + Credentials management: **Self managed**
     + Master password: **<_your-password_>**
     + Confirm master password: **<_your-password_>**

![RDS](/images/3-createrds/015-createrds.png?width=40pc)

   + Connectivity:
     + VPC: **workshop-1-vpc**
     + DB subnet group: **workshop-1-db-subnet-group**
     + Public access: **Yes** (Temporary)
     + Existing VPC security groups: **Database-SG**
     + Availability Zone: **us-east-1a** (or us-east-1b)

![RDS](/images/3-createrds/016-createrds.png?width=40pc)
![RDS](/images/3-createrds/017-createrds.png?width=40pc)

   + Monitoring:
     + Turn off Performance Insights

![RDS](/images/3-createrds/018-createrds.png?width=40pc)

3. Click **Create database** after reviewing all the information

![RDS](/images/3-createrds/019-createrds.png?width=40pc)

4. Wait for a few minutes to initilize the database instance

![RDS](/images/3-createrds/020-createrds.png?width=90pc)

{{% notice note %}}
Save the endpoint of Database instance for later usage.
{{% /notice %}}

#### Create Tables and inject data into database

In this workshop, I will use SQL Server Management Studio to interact with the database server. 

1. Download and install [SQL Server Managerment Studio](https://aka.ms/ssmsfullsetup)

![RDS](/images/3-createrds/28-additional.png?width=50pc)

2. Open SSMS, click to **File** and then choose **Connect Object Explorer**. After that, configure as follow
   + Server type: **Database Engine**
   + Server name: **<_database_endpoint_>,<_port_>**
      - E.g. **online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com,1433**
   + Authentication: SQL Server Authentication
     + Login: **admin** (Change if you use another username when create AWS RDS instance)
     + Password: **<_your_password_>**
   + Connection Security
     + Encryption: Mandatory
     + Check **Trust server certificate**
   + Click **Connect**

![RDS](/images/3-createrds/021-createrds.png?width=40pc)

{{% notice note %}}
When we create a SQL Server DB instance, AWS RDS creates an SSL certificate for it to ensure that data transmitted between client and database is encrypted. Consequently, we have to enable Trust Server Certificate to tell the client to trust the SSL/TLS certificate provided by the database server or we will end up with a certificate validation error.
{{% /notice %}}

3. Create a new database
   + After connecting to the database instance, right click to the **Databases** folder and choose **New database**. 
   + Then, create a Database with name **OnlineShopDB** and click **OK**

![RDS](/images/3-createrds/022-createrds.png?width=90pc)

4. Create table and inject data to database
   + Download **table-init.sql** and **data-init.sql** in the attachments below
   + In the menu bar of SSMS tool, choose Open File
   + Open the **table-init.sql** file
   + Click **Execute** button
   + Choose Open File again
   + Open the **data-init.sql** file
   + Click **Execute** button

![RDS](/images/3-createrds/024-createrds.png?width=90pc)
![RDS](/images/3-createrds/025-createrds.png?width=90pc)

{{%attachments style="orange" title="Attachments" pattern=".*(sql)"/%}}

5. Retrieve data from tables to check the result

```
SELECT TOP (1000) [ProductID]
   ,[Name]
   ,[Brand]
   ,[Description]
   ,[ImageURL]
FROM [OnlineShopDB].[dbo].[PRODUCT]
```
Product ID table:
![RDS](/images/3-createrds/026-createrds.png?width=90pc)

```
SELECT TOP (1000) [ProductSizeID]
   ,[Size]
   ,[Price]
   ,[Quantity]
   ,[ProductID]
FROM [OnlineShopDB].[dbo].[PRODUCT_SIZE]
```
Product Size table:
![RDS](/images/3-createrds/027-createrds.png?width=90pc)

{{% notice warning %}}
Remember to change the Publicibly accessible of Database instance to **No** and remove the route to Internet Gateway of **workshop-1-rds-rt** route table after we finish initilizing the database server.
{{% /notice %}}