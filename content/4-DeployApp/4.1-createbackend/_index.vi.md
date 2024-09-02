---
title : "Triển khai Online Shop Backend (.NET Core 6)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tạo EC2 Instance cho Backend
1. Ở trang chính của AWS Management Console, gõ **EC2** vào thanh tìm kiếm và chọn dịch vụ AWS EC2
   
![EC2](/images/4-createec2/001-createec2.png?width=50pc)

2. Tại trang chính của EC2
    + Chọn **Instances** ở bên thanh menu bên trái
    + Nhấp vào **Launch instances**

![EC2](/images/4-createec2/002-createec2.png?width=90pc)

3. Điền các thông tin sau cho EC2 instance
    + Name: ```Online-Shop-Backend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Vì ta dùng EC2 Instance Connect Endpoint)
    + VPC: **workshop-1-vpc**
    + Subnet: **workshop-1-subnet-private3-us-east-1a**
    + Auto-assign public IP: **Disable**
    + Common security groups: **Backend-SG**
    + Storage: **15 GiB gp2**

![EC2](/images/4-createec2/003-createec2.png?width=40pc)
![EC2](/images/4-createec2/004-createec2.png?width=40pc)
![EC2](/images/4-createec2/005-createec2.png?width=40pc)
![EC2](/images/4-createec2/006-createec2.png?width=40pc)
![EC2](/images/4-createec2/007-createec2.png?width=40pc)

4. Chọn Online-Shop-Backend instance và chọn **Connect**

![EC2](/images/4-createec2/008-createec2.png?width=90pc)

5. Chọn EIC endpoint mà chúng ta đã tạo trước đó và chọn **Connect**

![EC2](/images/4-createec2/009-createec2.png?width=40pc)


#### Tải các phần mềm cần thiết để triển khai ứng dụng backend
1. Sau khi truy cập vào giao diện terminal của EC2 instance, ta sẽ tiến hành cập nhật APT package manager và tải một số thư viện và phần mềm cần thiết dưới đây

```bash
# Cập nhật và tải phần mềm
sudo apt-get update -y
sudo apt-get install -y net-tools iputils-ping git vim docker.io
```

2. Tải mssql-tool để kiểm tra kết nối với AWS RDS

```bash
# Tải mssql-tool
# Thêm GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc

# Đăng ký the Microsoft Ubuntu repository (Ubuntu 22)
curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
sudo apt-get update
sudo apt-get install -y mssql-tools18 unixodbc-dev

# Tự động hêm sqlcmd và bcp vào biến môi trường mỗi khi đăng nhập vào shell
echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bash_profile
source ~/.bash_profile
```

Chọn "**Yes**" để chấp nhận liscense terms của mssql-tool.
![EC2](/images/4-createec2/010-createec2.png?width=90pc)
![EC2](/images/4-createec2/011-createec2.png?width=90pc)

3. Kiểm tra kết nối với cơ sở dữ liệu
   
Thay đổi các giá trị sau:
+ Database DNS: **<_database_endpoint_>**
+ Database username: **<_username_>**
+ Database user password: **<_password_>**

```bash
sqlcmd -C -S <database_endpoint> -U <username> -P <password>
1> USE OnlineShopDB;
2> GO
1> SELECT * FROM PRODUCT;
2> GO
```

![EC2](/images/4-createec2/012-createec2.png?width=90pc)

#### Triển khai ứng dụng backend

1. Clone source code từ Github và tạo Dockerfile bằng VIM editor

```bash
git clone https://github.com/lth216/Online-Shop-Backend.git
cd Online-Shop-Backend/
vi Dockerfile
```

Dockerfile
```Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /app
COPY . .
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS deploy
WORKDIR /deploy
ARG db_server
ENV ConnectionStrings__UserAppCon=$db_server
RUN useradd -r backend && chown -R backend. /deploy
USER backend
COPY --from=build --chown=backend:backend /app/out/ /deploy/
EXPOSE 5214
ENTRYPOINT ["dotnet", "backend.dll", "--urls", "http://0.0.0.0:5214"]
```

{{% notice tip %}}
Khi tạo Docker image bằng Dockerfile, ta nên áp dụng một số best practices như sử dụng multi-stage, base image với size nhỏ và tránh chạy Docker container với user root. Do đó ở Docker image này, chúng ta sẽ chia quá trình thành 2 giai đoạn: build và deploy. Ở giai đoạn deploy, ta sẽ tạo một người dùng "backend" là người dùng mặc định (non-root user) khi truy cập vào bên trong Docker container.
{{% /notice %}}

4. Xây dựng Docker image

Thay đổi các giá trị sau:
+ Database DNS: **<_database_endpoint_>**
+ Database username: **<_username_>**
+ Database user password: **<_password_>**

```bash
sudo docker build -t online-shop-be:1.0.0 --build-arg=db_connect="Server=<database_endpoint>,1433;Database=OnlineShopDB;User Id=<username>;Password=<password>" .
```

![EC2](/images/4-createec2/013-createec2.png?width=90pc)

5. Chạy Docker container
```bash
sudo docker run --name online-shop-backend --restart=always -p 5214:5214 -dt online-shop-be:1.0.0
```
Tên Docker image và Docker container có thể thay đổi theo ý thích của bạn.