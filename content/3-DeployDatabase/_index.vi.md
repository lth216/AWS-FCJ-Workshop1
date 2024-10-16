---
title : "Triển khai Database server"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Tạo cơ sở dữ liệu với AWS RDS
{{% notice note %}}
Khi triển khai cơ sở dữ liệu, ta nên triển khai một hoặc nhiều phiên bản DB dự phòng trên các Availability Zones khác nhau, để giảm tải công việc trên phiên bản DB chính và cung cấp tính sẵn có cao và hỗ trợ chuyển đổi dự phòng cho hệ thống cơ sở dữ liệu của chúng ta. Tuy nhiên, trong workshop này, chúng ta chỉ triển khai một phiên bản DB chính với Single-AZ (1 Availability Zone) để giảm chi phí.
{{% /notice %}}

Tham khảo chi phí cho MSSQL tại đây: [AWS MSSQL Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)

#### Tạo Subnet Group
{{% notice info %}}
Dịch vụ AWS RDS chọn 1 subnet và 1 địa chỉ IP nằm trong Subnet Group mà ta sắp sửa tạo sau đây để gán với Cơ sở dữ liệu. Và cơ sở dữ liệu đó sẽ được triển khai ở vùng khả dụng ứng với subnet mà ta đã gán vào. 
{{% /notice %}}

1. Gõ **RDS** vào thanh tìm kiếm của AWS Management Console và chọn dịch vụ AWS RDS

   ![RDS](/images/3-database/001-rds.png?width=90pc)

2. Ở trang chính của AWS RDS, chọn Subnet groups và nhấp vào **Create DB subnet group**

   ![RDS](/images/3-database/002-rds.png?width=90pc)

3. Điền các thông tin sau cho Subnet group
    + Name: ```workshop-1-db-subnet-group```
    + Description: ```Subnet group for database instance```
    + VPC: **workshop-1-vpc**
      ![RDS](/images/3-database/003-rds.png?width=90pc)
    + Availability Zones: **us-east-1a** và **us-east-1b**
    + Subnets: Chọn **private-subnet-rds-1** (10.26.208.0/20) và **private-subnet-rds-2** (10.26.192.0/20)
    + Nhấp chọn **Create**
      ![RDS](/images/3-database/004-rds.png?width=90pc)

4. Kiểm tra kết quả

   ![RDS](/images/3-database/005-rds.png?width=90pc)

#### Tạo cơ sở dữ liệu

1. Chọn **Databases** ở thanh công cụ bên trái và chọn **Create database**

   ![RDS](/images/3-database/006-rds.png?width=90pc)

2. Điền các thông tin sau cho cơ sở dữ liệu
   + Database creation method: **Standard create**
   + Engine options: **Microsoft SQL Server**
   + Database management type: **Amazon RDS**
   + Edition: **SQL Server Express Edition** (SQL Server Express Edition hỗ trợ Free tier)
   + Templates: **Free tier**

![RDS](/images/3-createrds/011-createrds.png?width=40pc)
![RDS](/images/3-createrds/012-createrds.png?width=40pc)
![RDS](/images/3-createrds/013-createrds.png?width=40pc)
![RDS](/images/3-createrds/014-createrds.png?width=40pc)

   + Mục Settings:
     + DB instance identifier: ```online-shop-database```
     + Master username: ```admin```
     + Credentials management: **Self managed**
     + Master password: **<_your-password_>**
     + Confirm master password: **<_your-password_>**

![RDS](/images/3-createrds/015-createrds.png?width=40pc)

   + Mục Connectivity:
     + VPC: **workshop-1-vpc**
     + DB subnet group: **workshop-1-db-subnet-group**
     + Public access: **Yes** (Lựa chọn tạm thời để cấu hình cơ sở dữ liệu bằng GUI)
     + Existing VPC security groups: **Database-SG**
     + Availability Zone: **us-east-1a** (hoặc us-east-1b)

![RDS](/images/3-createrds/016-createrds.png?width=40pc)
![RDS](/images/3-createrds/017-createrds.png?width=40pc)

   + Mục Monitoring:
     + Tắt Performance Insights vì chưa dùng tới

![RDS](/images/3-createrds/018-createrds.png?width=40pc)

3. Nhấp chọn **Create database** sau khi đã kiểm tra lại các cấu hình

![RDS](/images/3-createrds/019-createrds.png?width=40pc)

4. Đợi một vài phút để AWS tiến hành khởi tạo CSDL 

![RDS](/images/3-createrds/020-createrds.png?width=90pc)

{{% notice note %}}
Chúng ta sẽ lưu endpoint của Database instance lại để sử dụng cho các bước sau.
{{% /notice %}}

#### Tạo bảng trong Cơ sở dữ liệu

Chúng ta sẽ dùng tool có tên là SQL Server Management Studio để cấu hình cho Cơ sở dữ liệu mà ta vừa tạo. 

1. Tải và cài đặt [SQL Server Managerment Studio](https://aka.ms/ssmsfullsetup)

   ![RDS](/images/3-database/006-rds.png?width=90pc)

2. Cấu hình cho Database
   + Database creation method: **Standard create**
   + Engine options: **Microsoft SQL Server**
      ![RDS](/images/3-database/007-rds.png?width=90pc)
   + Database management type: **Amazon RDS**
   + Edition: **SQL Server Express Edition** (hỗ trợ Account Free tier)
   + Templates: **Free tier**
      ![RDS](/images/3-database/008-rds.png?width=90pc)
   + DB instance identifier: ```online-shop-database```
   + Master username: ```admin```
   + Credentials management: **Self managed**
   + Master password: (Điền mật khẩu của bạn)
   + Confirm master password: (Điền mật khẩu của bạn)
      ![RDS](/images/3-database/009-rds.png?width=90pc)
   + VPC: **workshop-1-vpc**
   + DB subnet group: **workshop-1-db-subnet-group**
   + Public access: **No**
      ![RDS](/images/3-database/010-rds.png?width=90pc)
   + Chọn Existing VPC security groups: **Database-SG**
   + Availability Zone: **us-east-1a** (or **us-east-1b**)
      ![RDS](/images/3-database/011-rds.png?width=90pc)
   + Tắt tính năng Performance Insights ở mục Monitoring
      ![RDS](/images/3-database/012-rds.png?width=90pc)
   + Nhấp chọn **Create database**
      ![RDS](/images/3-database/013-rds.png?width=90pc)

3. Đợi một vài phút để tiến hành khởi tạo Database
   + Lưu Database endpoint để sử dụng ở các bước sau.
   ![RDS](/images/3-database/014-rds.png?width=90pc)