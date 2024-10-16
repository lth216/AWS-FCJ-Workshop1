---
title : "Deploy Online Shop Frontend (ReactJS)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

#### Create EC2 Instance for Frontend application
1. In the AWS Management Console, type **EC2** in the search bar and choose EC2 service
    ![FE](/images/5-frontend/5.1-ec2/001-fe.png?width=90pc)

2. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu
    + Click **Launch instances**
    ![FE](/images/5-frontend/5.1-ec2/002-fe.png?width=90pc)

3. Specify details for Frontend EC2 Instance
    + Name: ```Online-Shop-Frontend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
        ![FE](/images/5-frontend/5.1-ec2/003-fe.png?width=90pc)
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Since we use EC2 Instance Connect Endpoint)
        ![FE](/images/5-frontend/5.1-ec2/004-fe.png?width=90pc)
    + VPC: **workshop-1-vpc**
    + Subnet: **workshop-1-subnet-private1-us-east-1a**
    + Auto-assign public IP: **Disable**
    + Common security groups: **Frontend-SG**
    + Storage: **30 GiB gp2**
        ![FE](/images/5-frontend/5.1-ec2/005-fe.png?width=90pc)

4. Wait until the status of EC2 Instance is <span style="color:green">Running</span>
    ![FE](/images/5-frontend/5.1-ec2/006-fe.png?width=90pc)

#### Connect to EC2 Instance by EIC
1. Choose the Online-Shop-Frontend instance and click **Connect**
    ![FE](/images/5-frontend/5.1-ec2/007-fe.png?width=90pc)

2. Specify the EIC endpoint that we already created to the EC2 Instance Connect Endpoint and click **Connect**
    ![FE](/images/5-frontend/5.1-ec2/008-fe.png?width=90pc)

3. Wait a few second until the connection completes
    ![FE](/images/5-frontend/5.1-ec2/009-fe.png?width=90pc)

#### Deploy Frontend application
1. Update APT package manager
    ```bash
    sudo apt-get update -y
    sudo apt-get install -y net-tools iputils-ping git vim
    ```

2. Install Docker with latest version
    ```bash
    curl -fsSL https://get.docker.com/ | bash
    ```

3. Clone source code from Github
    ```bash
    git clone https://github.com/lth216/Online-Shop-Frontend.git
    ```

4. Open file variables.js and replace default IP and Port with ```api.longtruong-lab.online``` hostname (Change the hostname with your own domain)
    ```bash
    cd Online-Shop-Frontend/src/utils/api
    vi variables.js
    ```
    ![FE](/images/5-frontend/5.1-ec2/010-fe.png?width=90pc)

5. Create Dockerfile for Frontend application
{{% notice tip %}}
In this Dockerfile, we will apply some best practices to enhance the security as well as minimize the size of the Docker image by splitting it into multistage and using a service user instead of root user.\
Reference for more strategies to optimize Docker container: [Optimize Docker container](https://devopsedu.vn/cac-phuong-phap-bao-mat-container-thuc-te/)
{{% /notice %}}
    ```bash
    cd ../../../
    vi Dockerfile
    ```

    Copy the content below and paste to Dockerfile
    ```Dockerfile
    FROM node:alpine3.19 AS build
    WORKDIR /app
    COPY package.json .
    RUN npm install --force
    COPY . .
    RUN npm run build

    FROM nginxinc/nginx-unprivileged:alpine3.19-perl AS deploy
    COPY --from=build /app/build /usr/share/nginx/html
    COPY online-shop-frontend.conf /etc/nginx/conf.d/
    EXPOSE 3000
    CMD ["nginx", "-g", "daemon off;"]
    ```

6. Enable port 3000 of Nginx Webserver
{{% notice note %}}
By default, the nginx-unprivileged image listens at port 8080 as described [here](https://hub.docker.com/r/nginxinc/nginx-unprivileged). However, our Frontend application listens at port 3000 so that we will create a Nginx configuration file called online-shop-frontend.conf to enable port 3000 for the frontend.
{{% /notice %}}
    ```bash
    vi online-shop-frontend.conf
    ```

    Copy the content below and paste to online-shop-frontend.conf file
    ```bash
    server {
        listen      3000;
        server_name localhost;
        location / {
            root    /usr/share/nginx/html;
            index   index.html index.html;
            try_files $uri /index.html;
        }
        error_page  500 502 503 504 /50x.html;
        location = /50x.html {
            root    /usr/share/nginx/html;
        }
    }
    ```

7. Build Docker image

    ```bash
    sudo docker buildx build -t online-shop-fe:1.0.0 .
    ```
    ![FE](/images/5-frontend/5.1-ec2/011-fe.png?width=90pc)

8. Run Docker container
    ```bash
    sudo docker run --name online-shop-frontend --security-opt="no-new-privileges=true" --restart=always -p 3000:3000 -dt online-shop-fe:1.0.0
    ```
    ![FE](/images/5-frontend/5.1-ec2/012-fe.png?width=90pc)
