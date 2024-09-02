---
title : "Configure Route Table for Database subnet"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### Configure Route Tables for database subnet

{{% notice note %}}
Usually, we put our database in a private subnet that blocks connections from the Internet. However, for the first database server, we need to work with the database to create it and set up the necessary tables. To make this process easier, I will set up a route table that allows connections from my local IP address so I can connect to the database from my computer using a tool called SQL Server Management Studio (SSMS).
{{% /notice %}}

1. In the VPC Management Dashboard
    + Select **Route tables** in the left menu.
    + Choose **Create route table**.

![VPC](/images/2-createvpc/011-routetable.png?width=90pc)

2. Configure Route table as below
    + Route table name: ```workshop-1-rds-rt```
    + VPC: **workshop-1-vpc**
    + Click **Create route table**

![VPC](/images/2-createvpc/012-routetable.png?width=40pc)

3. Edit route of the newly created route table
    + Click to the related route table
    + Choose **Edit routes** in the Routes section

![VPC](/images/2-createvpc/013-routetable.png?width=90pc)

4. Add route
    + Destination: ```0.0.0.0/0```
    + Target: ```workshop-1-igw``` (Internet Gateway of workshop-1 VPC) 
    + Then, click **Save changes**

![VPC](/images/2-createvpc/014-routetable.png?width=90pc)

{{% notice note %}}
To allow only connections from our local IP address, we will configure rules of Security Group in the next section.
{{% /notice %}}

5. Attach related subnets to the route table

- Change to **Subnet associations** section of the Route table and choose **Edit subnet associations**

![VPC](/images/2-createvpc/015-routetable.png?width=90pc)

- Choose **private-subnet-rds-1** and **private-subnet-rds-2**. Then, click **Save associations**

![VPC](/images/2-createvpc/016-routetable.png?width=90pc)

6. Verify the result

![VPC](/images/2-createvpc/017-routetable.png?width=90pc)