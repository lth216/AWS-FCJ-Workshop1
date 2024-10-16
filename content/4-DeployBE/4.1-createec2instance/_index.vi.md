---
title : "Triển khai Online Shop Backend (.NET Core 6)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tạo EC2 Instance cho Backend
1. Ở trang chính của AWS Management Console, gõ **EC2** vào thanh tìm kiếm và chọn dịch vụ AWS EC2
    ![BE](/images/4-backend/4.1-ec2/001-be.png?width=90pc)

2. Tại trang chính của EC2
    + Chọn **Instances** ở bên thanh menu bên trái
    + Nhấp vào **Launch instances**
    ![BE](/images/4-backend/4.1-ec2/002-be.png?width=90pc)

3.Cấu hình cho Backend EC2 Instance
    + Name: ```Online-Shop-Backend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
        ![BE](/images/4-backend/4.1-ec2/003-be.png?width=90pc)
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Vì chúng ta dùng EC2 Instance Connect Endpoint nên không cần phải tạo SSH key)
        ![BE](/images/4-backend/4.1-ec2/004-be.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Subnet: **workshop-1-subnet-private3-us-east-1a**
    + Auto-assign public IP: **Disable**
    + Common security groups: **Backend-SG**
    + Storage: **30 GiB gp2** 
        ![BE](/images/4-backend/4.1-ec2/005-be.png?width=90pc)

4. Đợi tới khi trạng thái của EC2 Instance là <span style="color:green">Running</span>
    ![BE](/images/4-backend/4.1-ec2/006-be.png?width=90pc)

#### Kết nối tới EC2 Instance thông qua EC2 Instance Connect Endpoint
1. Chọn Online-Shop-Backend instance và chọn **Connect**

    ![BE](/images/4-backend/4.1-ec2/007-be.png?width=90pc)

2. Chỉ định EIC endpoint mà chúng ta đã tạo, sau đó chọn **Connect**

    ![BE](/images/4-backend/4.1-ec2/008-be.png?width=90pc)

3. Đợi vài giây để tiến hành kết nối tới terminal của EC2 Instance
    ![BE](/images/4-backend/4.1-ec2/009-be.png?width=90pc)

#### Tải những thư viện cần thiết cho máy ảo dùng để triển khai Backend 
1. Cập nhật APT package manager và tải thư viện cần thiết
    ```bash
    sudo apt-get update -y
    sudo apt-get install -y net-tools iputils-ping git vim unzip
    ```

2. Tải version mới nhất của Docker
    ```bash
    curl -fsSL https://get.docker.com/ | bash
    ```

3. Tải mssql-tool
    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc
    curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
    sudo apt-get update
    sudo apt-get install -y mssql-tools18 unixodbc-dev
    ```
    Chọn "**Yes**" để đồng ý quy định và giấy phép của mssql-tool.
    ![BE](/images/4-backend/4.1-ec2/010-be.png?width=90pc)
    ![BE](/images/4-backend/4.1-ec2/011-be.png?width=90pc)

4. Kích hoạt sqlcmd
    ```bash
    echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bash_profile
    source ~/.bash_profile
    ```
5. Tải version mới nhất của AWS CLI
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    rm awscliv2.zip
    sudo ./aws/install
    ```
    
#### Tải file từ S3 Bucket
1. Tại trang chủ của EC2
   + Nhấp chọn Online-Shop-Backend
   + Tại mục Actions, chọn Modify IAM role ở phần Security
   ![BE](/images/4-backend/4.1-ec2/012-be.png?width=90pc)

2. Gán **AllowEC2ReadS3** IAM Role vào EC2 Instance. Sau đó, nhấp chọn **Update IAM role**.
    ![BE](/images/4-backend/4.1-ec2/013-be.png?width=90pc)

3. Chạy các câu lệnh dưới đây để copy file từ S3 Bucket
    + Liệt kê các objects ở **my-online-shop-bucket** bucket
        ```bash
        aws s3 ls my-online-shop-bucket
        ```
    + Copy file data-init.sql file tại thư mục hiện tại trên EC2
        ```bash
        aws s3 cp s3://my-online-shop-bucket/data-init.sql .
        ```
    + Copy file table-init.sql file tại thư mục hiện tại trên EC2
        ```bash
        aws s3 cp s3://my-online-shop-bucket/table-init.sql .
        ```
    ![BE](/images/4-backend/4.1-ec2/014-be.png?width=90pc)

#### Khởi tạo table và một số data có sẫn cho Database server
1. Lấy Database endpoint từ AWS RDS
    ![BE](/images/4-backend/4.1-ec2/015-be.png?width=90pc)

2. Tạo bảng và cập nhật một số data
    + Dùng câu lệnh sau để kết nối với Database server (Hãy thay đổi Database endpoint tương ứng với trường hợp của bạn)
    + Bạn sẽ cần phải nhập password cho admin user mà bạn đã tạo ở bước trước.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin
        ```
    + Tạo Database tên là ```OnlineShopDB```
        ```bash
        1> CREATE DATABASE OnlineShopDB;
        2> GO
        1> quit
        ```
    + Tạo bảng trong Database bằng file **table-init.sql**
    + Bạn sẽ cần phải nhập password cho admin user mà bạn đã tạo ở bước trước.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin -d OnlineShopDB -i table-init.sql
        ```
    + Lưu một vài data vào trong Database bằng file **data-init.sql** 
    + Bạn sẽ cần phải nhập password cho admin user mà bạn đã tạo ở bước trước.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin -d OnlineShopDB -i data-init.sql
        ```
    ![BE](/images/4-backend/4.1-ec2/016-be.png?width=90pc)
    
3. Lấy thông tin từ Database để kiểm tra thử
    ```bash
    sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin
    #Password: 
    1> USE OnlineShopDB;
    2> GO
    1> SELECT * FROM PRODUCT;
    2> GO
    ```
    ![BE](/images/4-backend/4.1-ec2/017-be.png?width=90pc)


#### Triển khai ứng dụng Backend
1. Lấy source code từ GitHub và tạo Dockerfile bằng vim editor
{{% notice tip %}}
Trong Dockerfile này, chúng ta sẽ áp dụng một số quy tắc tốt nhất để tăng cường bảo mật cũng như giảm kích thước của hình ảnh Docker bằng cách chia thành nhiều giai đoạn và sử dụng người dùng dịch vụ thay vì người dùng root.\
Tham khảo thêm các phương pháp để tối ưu Docker image: [Optimize Docker container](https://devopsedu.vn/cac-phuong-phap-bao-mat-container-thuc-te/)
{{% /notice %}}
    ```bash
    git clone https://github.com/lth216/Online-Shop-Backend.git
    cd Online-Shop-Backend/
    vi Dockerfile
    ```

    Copy nội dung sau và dán vào file Dockerfile
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

2. Tạo Docker image
    + Thay đổi Database endpoint và password ở trường hợp của bạn
    ```bash
    sudo docker buildx build -t online-shop-be:1.0.0 --build-arg=db_server="Server=online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com,1433;Database=OnlineShopDB;User Id=admin;Password=Thl12345" .
    ```

    ![BE](/images/4-backend/4.1-ec2/018-be.png?width=90pc)

3. Chạy Docker container
    ```bash
    sudo docker run --name online-shop-backend --security-opt="no-new-privileges=true" --restart=always -p 5214:5214 -dt online-shop-be:1.0.0
    ```
4. Kiểm tra kết quả
    ```bash
    sudo docker ps
    ```
    ![BE](/images/4-backend/4.1-ec2/019-be.png?width=90pc)