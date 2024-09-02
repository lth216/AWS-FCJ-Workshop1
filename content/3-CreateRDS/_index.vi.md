---
title : "Tạo cơ sở dữ liệu với AWS RDS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Tạo cơ sở dữ liệu với AWS RDS
{{% notice note %}}
Trong phạm vi của workshop này, ta sẽ triển khai cơ sở dữ liệu với Single-AZ để tiết kiệm chi phí và tận dụng tài khoản Free Tier (nếu có). Tuy nhiên, trong thực tế, chúng ta nên triển khai cơ sở dữ liệu với Multi-AZ để tăng tính sẵn sàng cao cho cơ sở dữ liệu
{{% /notice %}}

Tham khảo chi phí cho MSSQL tại đây: [AWS MSSQL Pricing](https://aws.amazon.com/rds/sqlserver/pricing/)

#### Tạo Subnet Group
{{% notice info %}}
Dịch vụ AWS RDS chọn 1 subnet và 1 địa chỉ IP nằm trong Subnet Group mà ta sắp sửa tạo sau đây để gán với Cơ sở dữ liệu. Và cơ sở dữ liệu đó sẽ được triển khai ở vùng khả dụng ứng với subnet mà ta đã gán vào. 
{{% /notice %}}

1. Gõ **RDS** vào thanh tìm kiếm của AWS Management Console và chọn dịch vụ AWS RDS

![RDS](/images/3-createrds/001-createrds.png?width=50pc)

2. Ở trang chính của AWS RDS, chọn Subnet groups và nhấp vào **Create DB subnet group**

![RDS](/images/3-createrds/002-createrds-1.png?width=90pc)

3. Điền các thông tin sau cho Subnet group
    + Name: ```workshop-1-db-subnet-group```
    + Description: ```Subnet group for database instance```
    + VPC: **workshop-1-vpc**
    + Availability Zones: **us-east-1a** và **us-east-1b**
    + Subnets: **private-subnet-rds-1** và **private-subnet-rds-2**
    + Nhấp chọn **Create**

![RDS](/images/3-createrds/003-createrds.png?width=40pc)
![RDS](/images/3-createrds/004-createrds.png?width=40pc)

4. Kiểm tra kết quả

![RDS](/images/3-createrds/005-createrds.png?width=90pc)

#### Tạo cơ sở dữ liệu

1. Chọn **Databases** ở thanh công cụ bên trái và chọn **Create database**

![VPC](/images/3-createrds/006-createrds-1.png?width=90pc)

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

![RDS](/images/3-createrds/28-additional.png?width=50pc)

2. Mở SSMS, Nhập vào **File** trên thành công cụ và chọn **Connect Object Explorer**. Sau đó, nhập các thông tin sau để kết nối với CSDL trên AWS RDS
   + Server type: **Database Engine**
   + Server name: **<_database_endpoint_>,<_port_>** (Sử dụng Database endpoint mà ta đã lưu ở bước trước, port mặc định là 1433).
      - Ví dụ: **online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com,1433**
   + Authentication: SQL Server Authentication
     + Login: **admin**
     + Password: **<_your_password_>**
   + Connection Security
     + Encryption: Mandatory
     + Nhấp chọn **Trust server certificate**
   + Chọn **Connect**

![RDS](/images/3-createrds/021-createrds.png?width=40pc)

{{% notice note %}}
Khi chúng ta tạo một phiên bản SQL Server DB, AWS RDS sẽ tạo một chứng chỉ SSL để đảm bảo dữ liệu được truyền giữa máy khách và cơ sở dữ liệu được mã hóa. Do đó, chúng ta phải bật Trust Server Certificate để thông báo cho máy khách tin tưởng chứng chỉ SSL/TLS do máy chủ cơ sở dữ liệu cung cấp, nếu không sẽ gặp lỗi xác thực chứng chỉ.
{{% /notice %}}

3. Tạo database
   + Sau khi kết nối tới CSDL trên AWS, nhấp chuột phải vào folder **Databases** và chọn **New database**. 
   + Sau đó tạo một database mới với tên ```OnlineShopDB``` và nhấp chọn **OK**

![RDS](/images/3-createrds/022-createrds.png?width=90pc)

4. Tạo bảng và thêm một số data có sẵn
   + Tải 2 files **table-init.sql** và **data-init.sql** đính kèm phía dưới
   + Trên thanh công cụ của SSMS, chọn Open File
   + Mở file **table-init.sql**
   + Chọn **Execute**
   + Chọn Open File lần nữa
   + Mở file **data-init.sql**
   + Nhấp chọn **Execute**

![RDS](/images/3-createrds/024-createrds.png?width=90pc)
![RDS](/images/3-createrds/025-createrds.png?width=90pc)

{{%attachments style="orange" title="Attachments" pattern=".*(sql)"/%}}

5. Kiểm tra data trong CSDL

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
Sau khi chúng ta hoàn thành việc khởi tạo máy chủ cơ sở dữ liệu, hãy thay đổi trạng thái Publicibly accessible của Database instance thành **No** và xóa đường đi tới Internet Gateway của bảng định tuyến **workshop-1-rds-rt** 
{{% /notice %}}