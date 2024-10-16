---
title : "Deploy Online Shop Backend (.NET Core 6)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Create EC2 Instance for Backend application
1. In the AWS Management Console, type **EC2** in the search bar and choose EC2 service
    ![BE](/images/4-backend/4.1-ec2/001-be.png?width=90pc)

2. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu
    + Click **Launch instances**
    ![BE](/images/4-backend/4.1-ec2/002-be.png?width=90pc)

3. Specify details for Backend EC2 Instance
    + Name: ```Online-Shop-Backend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
        ![BE](/images/4-backend/4.1-ec2/003-be.png?width=90pc)
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Since we use EC2 Instance Connect Endpoint)
        ![BE](/images/4-backend/4.1-ec2/004-be.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Subnet: **workshop-1-subnet-private3-us-east-1a**
    + Auto-assign public IP: **Disable**
    + Common security groups: **Backend-SG**
    + Storage: **30 GiB gp2** 
        ![BE](/images/4-backend/4.1-ec2/005-be.png?width=90pc)

4. Wait until the status of EC2 Instance is <span style="color:green">Running</span>
    ![BE](/images/4-backend/4.1-ec2/006-be.png?width=90pc)

#### Connect to EC2 Instance by EIC
1. Choose the Online-Shop-Backend instance and click **Connect**

    ![BE](/images/4-backend/4.1-ec2/007-be.png?width=90pc)

2. Specify the EIC endpoint that we already created to the EC2 Instance Connect Endpoint and click **Connect**

    ![BE](/images/4-backend/4.1-ec2/008-be.png?width=90pc)

3. Wait a few second until the connection completes
    ![BE](/images/4-backend/4.1-ec2/009-be.png?width=90pc)

#### Install necessary packages and dependencies
1. Update APT package manager and install necessary packages
    ```bash
    sudo apt-get update -y
    sudo apt-get install -y net-tools iputils-ping git vim unzip
    ```

2. Install latest version of Docker
    ```bash
    curl -fsSL https://get.docker.com/ | bash
    ```

3. Install mssql-tool
    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc
    curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
    sudo apt-get update
    sudo apt-get install -y mssql-tools18 unixodbc-dev
    ```
    Choose "**Yes**" to accept liscense terms of mssql-tool.
    ![BE](/images/4-backend/4.1-ec2/010-be.png?width=90pc)
    ![BE](/images/4-backend/4.1-ec2/011-be.png?width=90pc)

4. Make sqlcmd and bcp accessible from bash shell
    ```bash
    echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bash_profile
    source ~/.bash_profile
    ```
5. Install latest version of AWS CLI
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    rm awscliv2.zip
    sudo ./aws/install
    ```
    
#### Get objects from S3 Bucket
1. In the EC2 Management Dashboard
   + Tick to Online-Shop-Backend
   + Toggle Actions and choose Modify IAM role in Security section
   ![BE](/images/4-backend/4.1-ec2/012-be.png?width=90pc)

2. Attach the **AllowEC2ReadS3** IAM Role to the EC2 Instance. Then, click **Update IAM role**.
    ![BE](/images/4-backend/4.1-ec2/013-be.png?width=90pc)

3. Run the following commands to check and copy objects from S3 Bucket
    + List objects in my-online-shop-bucket
        ```bash
        aws s3 ls my-online-shop-bucket
        ```
    + Copy data-init.sql file to the current working directory
        ```bash
        aws s3 cp s3://my-online-shop-bucket/data-init.sql .
        ```
    + Copy table-init.sql file to the current working directory
        ```bash
        aws s3 cp s3://my-online-shop-bucket/table-init.sql .
        ```
    ![BE](/images/4-backend/4.1-ec2/014-be.png?width=90pc)

#### Initialize Database server
1. Get Database endpoint from AWS RDS
    ![BE](/images/4-backend/4.1-ec2/015-be.png?width=90pc)

2. Initialize Database server
    + Run the command to connect to Database server with mssql-tool (Note: change the database endpoint in your case)
    + You will be prompted to provide password of admin user.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin
        ```
    + Run these queries to create database with name ```OnlineShopDB```
        ```bash
        1> CREATE DATABASE OnlineShopDB;
        2> GO
        1> quit
        ```
    + Import file **table-init.sql** to create required tables
    + You will be prompted to provide password of admin user.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin -d OnlineShopDB -i table-init.sql
        ```
    + Import file **data-init.sql** to inject data into tables
    + You will be prompted to provide password of admin user.
        ```bash
        sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin -d OnlineShopDB -i data-init.sql
        ```
    ![BE](/images/4-backend/4.1-ec2/016-be.png?width=90pc)
    
3. Retrieve data from database server
    ```bash
    sqlcmd -C -S online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com -U admin
    #Password: 
    1> USE OnlineShopDB;
    2> GO
    1> SELECT * FROM PRODUCT;
    2> GO
    ```
    ![BE](/images/4-backend/4.1-ec2/017-be.png?width=90pc)


#### Deploy Backend application
1. Clone source code from Github and create Dockerfile by vim editor
{{% notice tip %}}
In this Dockerfile, we will apply some best practices to enhance the security as well as minimize the size of the Docker image by splitting it into multistage and using a service user instead of root user.\
Reference for more strategies to optimize Docker container: [Optimize Docker container](https://devopsedu.vn/cac-phuong-phap-bao-mat-container-thuc-te/)
{{% /notice %}}
    ```bash
    git clone https://github.com/lth216/Online-Shop-Backend.git
    cd Online-Shop-Backend/
    vi Dockerfile
    ```

    Copy the content below and paste to Dockerfile
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

2. Build Docker image
    + Change the database endpoint and password according to your use case
    ```bash
    sudo docker buildx build -t online-shop-be:1.0.0 --build-arg=db_server="Server=online-shop-database.cxuw0uag4tfy.us-east-1.rds.amazonaws.com,1433;Database=OnlineShopDB;User Id=admin;Password=Thl12345" .
    ```

    ![BE](/images/4-backend/4.1-ec2/018-be.png?width=90pc)

3. Run Docker container
    ```bash
    sudo docker run --name online-shop-backend --security-opt="no-new-privileges=true" --restart=always -p 5214:5214 -dt online-shop-be:1.0.0
    ```
4. Verify result
    ```bash
    sudo docker ps
    ```
    ![BE](/images/4-backend/4.1-ec2/019-be.png?width=90pc)