---
title : "Clean up"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 8. </b> "
---
#### Clean up
Remove the AWS resources in the following order:

#### Remove Auto-scaling group
- In the **EC2 Management Dashboard**.
- Choose the **Auto Scaling Groups** in the left menu.
- Select Auto Scaling Group: **Online-Shop-FE-ASG** and **Online-Shop-BE-ASG**.
- Click **Delete**.
- Type ```delete``` in the empty box and press **Delete**.

#### Remove Load Balancer
- In the **EC2 Management Dashboard**.
- Choose the **Load Balancers** in the left menu.
- Select **Online-Shop-FE-LB** and **Online-Shop-BE-LB**.
- Click **Actions**.
- Click **Delete load balancer**.
- Type ```confirm``` in the empty box and press **Delete**.

#### Remove Target groups
- In the **EC2 Management Dashboard**.
- Choose the **Target Groups** in the left menu.
- Select **Backend-TG** and **Frontend-TG**.
- Click **Actions**.
- Click **Delete**.
- Choose **Yes,delete**.

#### Remove Launch Template
- In the **EC2 Management Dashboard**.
- Choose the **Launch Templates** in the left menu.
- Select **Online-Shop-FE-launch-template** and **Online-Shop-BE-launch-template**.
- Click **Actions**.
- Click **Delete template**.
- Type ```Delete``` in the empty box and click **Delete**.

#### Remove AMI
- In the **EC2 Management Dashboard**.
- Select the **AMIs** in the left menu.
- Select **Online-Shop-FE-AMI** and **Online-Shop-BE-AMI**.
- Click **Actions**.
- Choose **Deregister AMI**.
- Choose **Deregister AMI** buttom.

#### Terminate EC2 instances
- In the **EC2 Management Dashboard**.
- Access the **Instances** in the left menu.
- Select all EC2 instances relating to the workshop: **Online-Shop-Frontend** and **Online-Shop-Backend**.
- Click **Actions**.
- Toggle **Instance State**.
- Select **Terminate (delete) instance**.

#### Delete DB instance
- Access AWS RDS Management Console.
- Select Databases in the left side bar.
- Select online-shop-database.
- Click Actions
- Click Delete
- Uncheck **Create final snapshot?** and select "**I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available**".
- Type ```delete me``` in the empty box and click **Delete**.

#### Delete EC2 Instance Connect Endpoint
- Access to VPC Management Console.
- Select the endpoint relating to the workshop.
- Click **Actions**.
- Click **Delete vpc endpoints**.
- Type ```delete``` in the box to confirm and click **Delete**.

#### Delete the VPC
- Access VPC Management Console.
- On the left navigation bar, select **Your VPCs**.
- Select the related VPC of this workshop: **workshop-1-vpc**.
- Click **Actions**.
- Click **Delete VPC**.
- Type ```delete``` in the field and click **Delete**.