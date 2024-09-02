---
title : "Triển khai Online Shop Frontend (ReactJS)"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

#### Tạo EC2 Instance cho Frontend application
1. Tại trang chính của AWS Console, gõ **EC2** vào thanh tìm kiếm và chọn EC2
   
![EC2](/images/4-createec2/001-createec2.png?width=50pc)

2. Tại trang chính của EC2
    + Chọn **Instances** thanh menu bên trái
    + Nhấp chọn **Launch instances**

![EC2](/images/4-createec2/002-createec2.png?width=90pc)

3. Cấu hình Frontend EC2 Instance như sau
    + Name: ```Online-Shop-Frontend```
    + AMI: **Ubuntu Server 22.04** (Free tier eligible)
    + Architecture: **64 bit (x86)**
    + Instance type: **t2.micro**
    + Key pair: **Proceed without a key pair** (Vì ta sẽ dùng EC2 Instance Connect Endpoint)
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

#### Tải các phần mềm cần thiết để triển khai ứng dụng frontend
1. Sau khi truy cập vào giao diện terminal của EC2 instance, ta sẽ tiến hành cập nhật APT package manager và tải một số thư viện và phần mềm cần thiết dưới đây

```bash
# Cập nhật và tải phần mềm
sudo apt-get update -y
sudo apt-get install -y net-tools iputils-ping git vim docker.io
```

2. Clone source code từ Github

```bash
git clone https://github.com/lth216/Online-Shop-Frontend.git
```

3. Thay thế địa chỉ IP và port 5214 thành DNS của Backend Load Balancer ở file variables.js. Điều này sẽ giúp giao diện web có thể giao tiếp với ứng dụng backend thông qua Backend Load Balancer.

```bash
cd Online-Shop-Frontend/
vi src/utils/api/variables.js
```

variables.js (Trước)
![EC2](/images/4-createec2/036-createec2.png?width=40pc)

variables.js (Sau)
![EC2](/images/4-createec2/037-createec2.png?width=40pc)


5. Tạo Dockerfile cho Frontend application

```bash
vi Dockerfile
```

Dockerfile
```Dockerfile
FROM node:alpine3.19 AS build
WORKDIR /app
COPY package.json .
RUN npm install --choce
COPY . .
RUN npm run build

FROM nginxinc/nginx-unprivileged:alpine3.19-perl AS deploy
COPY --from=build /app/build /usr/share/nginx/html
COPY online-shop-frontend.conf /etc/nginx/conf.d/
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
```

{{% notice note %}}
Mặc định, image nginxinc/nginx-unprivileged:alpine3.19-perl sử dụng port 8080 theo như mô tả ở đây [here](https://hub.docker.com/r/nginxinc/nginx-unprivileged) thay cho port 80 như image Nginx thông thường. Tuy nhiên, vì ứng dụng Frontend sử dụng port 3000 nên ta sẽ tạo 1 file cấu hình gọi là online-shop-frontend.conf để cấu hình port 3000 cho Nginx webserver.
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

6. Xây dựng Docker image

```bash
sudo docker build -t online-shop-fe:1.0.0 .
```

![EC2](/images/4-createec2/038-createec2.png?width=40pc)

7. Chạy Docker container
```bash
sudo docker run --name online-shop-frontend --restart=always -p 3000:3000 -dt online-shop-fe:1.0.0
```
![EC2](/images/4-createec2/039-createec2.png?width=90pc)

Tên Docker image và Docker container có thể thay đổi theo ý thích của bạn.