---
title : "Dọn dẹp tài nguyên"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

#### Dọn dẹp các tài nguyên
Dọn dẹp tài nguyên theo thứ tự sau đây:

#### Xóa Auto Scaling Group
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn **Auto Scaling Groups** phía bên menu trái.
- Chọn Auto Scaling Group: **Online-Shop-FE-ASG** và **Online-Shop-BE-ASG**.
- Nhấp chọn **Delete**.
- Gõ ```delete``` vào ô và bấm chọn **Delete**.

#### Xóa Load Balancer
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn the **Load Balancers** ở thanh menu trái
- Chọn **Online-Shop-FE-LB** và **Online-Shop-BE-LB**.
- Nhấp chọn **Actions**.
- Nhấp chọn **Delete load balancer**.
- Gõ ```confirm``` vào ô trống và chọn **Delete**.

#### Xóa Target groups
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn the **Target Groups** ở thanh menu trái
- Chọn **Backend-TG** và **Frontend-TG**.
- Nhấp chọn **Actions**.
- Nhấp chọn **Delete**.
- Chọn **Yes,delete**.

#### Xóa Launch Template
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn the **Launch Templates** ở thanh menu trái
- Chọn **Online-Shop-FE-launch-template** và **Online-Shop-BE-launch-template**.
- Nhấp chọn **Actions**.
- Nhấp chọn **Delete template**.
- Gõ ```Delete``` vào ô trống và Nhấp chọn **Delete**.

#### Xóa AMI
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn the **AMIs** ở thanh menu trái
- Chọn **Online-Shop-FE-AMI** và **Online-Shop-BE-AMI**.
- Nhấp chọn **Actions**.
- Chọn **Deregister AMI**.
- Chọn **Deregister AMI**.

#### Terminate EC2 instances
- Ở trang chính của **EC2 Management Dashboard**.
- Chọn **Instances** ở thanh menu trái
- Chọn các EC2 Instances của Workshop 1: **Online-Shop-Frontend** và **Online-Shop-Backend**.
- Nhấp chọn **Actions**.
- Mở rộng **Instance State**.
- Chọn **Terminate (delete) instance**.

#### Xóa DB instance
- Truy cập trang chính của AWS RDS Console.
- Chọn Databases ở menu trái.
- Chọn online-shop-database.
- Nhấp chọn Actions
- Nhấp chọn Delete
- Bỏ chọn **Create final snapshot?** và chọn "**I acknowledge that upon instance deletion, automated backups, including system snapshots và point-in-time recovery, will no longer be available**".
- Gõ ```delete me``` vào ô trống và Nhấp chọn **Delete**.

#### Xóa EC2 Instance Connect Endpoint
- Truy cập trang chính của VPC Management Console.
- Chọn EIC endpoint đã tạo.
- Nhấp chọn **Actions**.
- Nhấp chọn **Delete vpc endpoints**.
- Gõ ```delete``` vào ô trống và Nhấp chọn **Delete**.

#### Xóa VPC
- Truy cập trang chính của VPC Management Console.
- Chọn **Your VPCs** ở menu bên trái
- Chọn VPC: **workshop-1-vpc**.
- Nhấp chọn **Actions**.
- Nhấp chọn **Delete VPC**.
- Gõ ```delete``` vào ô trống và Nhấp chọn **Delete**.