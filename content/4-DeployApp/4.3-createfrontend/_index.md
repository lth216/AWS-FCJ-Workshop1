---
title : "Deploy Online Shop Frontend (ReactJS)"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

#### Create EC2 Instance for Frontend application
1. In the AWS Management Console, type **EC2** in the search bar and choose EC2 service
   
![EC2](/images/4-createec2/001-createec2.png?width=50pc)

2. In the EC2 Management Dashboard
    + Choose **Instances** in the left menu
    + Click **Launch instances**

![EC2](/images/4-createec2/002-createec2.png?width=90pc)

3. Specify details for Frontend EC2 Instance
    + Name: ```Online-Shop-Frontend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Since we use EC2 Instance Connect Endpoint)
    + VPC: **workshop-1-vpc**
    + Subnet: **workshop-1-subnet-private1-us-east-1a**
    + Auto-assign public IP: **Disable**
    + Common security groups: **Frontend-SG**
    + Storage: **15 GiB gp2** 

![EC2](/images/4-createec2/031-createec2.png?width=40pc)
![EC2](/images/4-createec2/032-createec2.png?width=40pc)
![EC2](/images/4-createec2/033-createec2.png?width=40pc)
![EC2](/images/4-createec2/034-createec2.png?width=40pc)
![EC2](/images/4-createec2/035-createec2.png?width=40pc)

#### Install necessary packages and dependencies for frontend application
1. After accessing to the EC2 Instance terminal, update APT package manager and install the following dependencies.

```bash
# Update and install dependencies
sudo apt-get update -y
sudo apt-get install -y net-tools iputils-ping git vim docker.io
```

2. Clone source code from Github

```bash
git clone https://github.com/lth216/Online-Shop-Frontend.git
```

3. Replace default IP address and port with the URL of Backend Load Balancer in file variables.js

```bash
cd Online-Shop-Frontend/
vi src/utils/api/variables.js
```

variables.js (before)
![EC2](/images/4-createec2/036-createec2.png?width=40pc)

variables.js (after)
![EC2](/images/4-createec2/037-createec2.png?width=40pc)


5. Create Dockerfile for Frontend application

```bash
vi Dockerfile
```

Dockerfile
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

{{% notice note %}}
By default, the nginxinc/nginx-unprivileged:alpine3.19-perl image listens at port 8080 as described [here](https://hub.docker.com/r/nginxinc/nginx-unprivileged). However, our Frontend application listens at port 3000 so that we will create a Nginx configuration file called online-shop-frontend.conf to enable port 3000 for the frontend.
{{% /notice %}}

```bash
vi online-shop-frontend.conf
```
online-shop-frontend.conf
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

6. Build Docker image

```bash
sudo docker build -t online-shop-fe:1.0.0 .
```

![EC2](/images/4-createec2/038-createec2.png?width=40pc)

7. Run Docker container
```bash
sudo docker run --name online-shop-frontend --restart=always -p 3000:3000 -dt online-shop-fe:1.0.0
```
![EC2](/images/4-createec2/039-createec2.png?width=90pc)

You can optionally change container name and image name based on your interest.