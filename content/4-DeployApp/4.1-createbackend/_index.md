---
title : "Deploy Online Shop Backend (.NET Core 6)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Create EC2 Instance for Backend application
1. In the AWS Management Console, type **EC2** in the search bar and choose EC2 service
   
![EC2](/images/4-createec2/001-createec2.png?width=50pc)

2. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu
    + Click **Launch instances**

![EC2](/images/4-createec2/002-createec2.png?width=90pc)

3. Specify details for Backend EC2 Instance
    + Name: ```Online-Shop-Backend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Since we use EC2 Instance Connect Endpoint)
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

4. Choose the Online-Shop-Backend instance and click **Connect**

![EC2](/images/4-createec2/008-createec2.png?width=90pc)

5. Specify the EIC endpoint that we already created to the EC2 Instance Connect Endpoint and click **Connect**

![EC2](/images/4-createec2/009-createec2.png?width=40pc)


#### Install necessary packages and dependencies for backend application
1. After accessing to the EC2 Instance terminal, update APT package manager and install the following dependencies

```bash
# Update and install dependencies
sudo apt-get update -y
sudo apt-get install -y net-tools iputils-ping git vim docker.io
```

2. Install mssql-tool to interact with database instance

```bash
# Install mssql-tool
# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc

# Register the Microsoft Ubuntu repository (Ubuntu 22)
curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list
sudo apt-get update
sudo apt-get install -y mssql-tools18 unixodbc-dev

# Make sqlcmd and bcp accessible from bash shell
echo 'export PATH="$PATH:/opt/mssql-tools18/bin"' >> ~/.bash_profile
source ~/.bash_profile
```
Choose "**Yes**" to accept liscense terms of mssql-tool.
![EC2](/images/4-createec2/010-createec2.png?width=90pc)
![EC2](/images/4-createec2/011-createec2.png?width=90pc)

3. Check connection to the Database server
   
Change the following data:
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

#### Deploy backend application

1. Clone source code from Github and create Dockerfile by vim editor

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
When create Docker image with Dockerfile, it is recommended to follow some best practices such as using multi-stage, minimal base image and avoid running Docker container as root user. That's why in this Docker image, we will split the process into 2 stages: build and deploy and create a "backend" user which is as default user when we access to inside the Docker container.
{{% /notice %}}

4. Build Docker image

Change the following data:
+ Database DNS: **<_database_endpoint_>**
+ Database username: **<_username_>**
+ Database user password: **<_password_>**

```bash
sudo docker build -t online-shop-be:1.0.0 --build-arg=db_connect="Server=<database_endpoint>,1433;Database=OnlineShopDB;User Id=<username>;Password=<password>" .
```

![EC2](/images/4-createec2/013-createec2.png?width=90pc)

5. Run Docker container
```bash
sudo docker run --name online-shop-backend --restart=always -p 5214:5214 -dt online-shop-be:1.0.0
```
You can optionally change container name and image name based on your interest.