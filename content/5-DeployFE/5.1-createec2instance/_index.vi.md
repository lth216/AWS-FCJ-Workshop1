---
title : "Triển khai Online Shop Frontend (ReactJS)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

#### Tạo EC2 Instance cho Frontend application
1. Tại trang chính của AWS Console, gõ **EC2** vào thanh tìm kiếm và chọn EC2
    ![FE](/images/5-frontend/5.1-ec2/001-fe.png?width=90pc)

2. Tại trang chính của EC2
    + Chọn **Instances** thanh menu bên trái
    + Nhấp chọn **Launch instances**
    ![FE](/images/5-frontend/5.1-ec2/002-fe.png?width=90pc)

3. Cấu hình Frontend EC2 Instance như sau
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

4. Chờ tới khi trạng thái của EC2 Instance là <span style="color:green">Running</span>
    ![FE](/images/5-frontend/5.1-ec2/006-fe.png?width=90pc)

#### Kết nối tới EC2 Instance của Frontend bằng EIC
1.Chọn vào Online-Shop-Frontend EC2 Instane và chọn **Connect**
    ![FE](/images/5-frontend/5.1-ec2/007-fe.png?width=90pc)

2. Chỉ EC2 Instance Connect Endpoint mà ta đã tạo sau đó nhấp chọn **Connect**
    ![FE](/images/5-frontend/5.1-ec2/008-fe.png?width=90pc)

3. Đợi vài giây để việc kết nối hoàn tất
    ![FE](/images/5-frontend/5.1-ec2/009-fe.png?width=90pc)

#### Triển khai ứng dụng Frontend
1. Cập nhật APT package manager
    ```bash
    sudo apt-get update -y
    sudo apt-get install -y net-tools iputils-ping git vim
    ```

2. Tải version mới nhất cho Docker
    ```bash
    curl -fsSL https://get.docker.com/ | bash
    ```

3. Clone source code từ Github
    ```bash
    git clone https://github.com/lth216/Online-Shop-Frontend.git
    ```

4. Mở file variables.js và thay thế địa chỉ IP và port mặc định với tên miền ```api.longtruong-lab.online``` (Thay đổi tên miền ở trường hợp của bạn)
    ```bash
    cd Online-Shop-Frontend/src/utils/api
    vi variables.js
    ```
    ![FE](/images/5-frontend/5.1-ec2/010-fe.png?width=90pc)


5. Tạo Dockerfile cho Frontend application
{{% notice tip %}}
Trong Dockerfile này, chúng ta sẽ áp dụng một số best practices để tăng cường bảo mật cũng như giảm kích thước của hình ảnh Docker bằng cách chia thành nhiều giai đoạn và sử dụng người dùng dịch vụ thay vì người dùng root..\
Tham khảo thêm các phương pháp để tối ưu Docker image: [Optimize Docker container](https://devopsedu.vn/cac-phuong-phap-bao-mat-container-thuc-te/)
{{% /notice %}}
    ```bash
    cd ../../../
    vi Dockerfile
    ```

    Copy nội dung sau và dán vào Dockerfile
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
Mặc định, image nginxinc/nginx-unprivileged:alpine3.19-perl sử dụng port 8080 theo như mô tả ở đây [here](https://hub.docker.com/r/nginxinc/nginx-unprivileged) thay cho port 80 như image Nginx thông thường. Tuy nhiên, vì ứng dụng Frontend sử dụng port 3000 nên ta sẽ tạo 1 file cấu hình gọi là online-shop-frontend.conf để cấu hình port 3000 cho Nginx webserver.
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

7. Xây dựng Docker image
    ```bash
    sudo docker buildx build -t online-shop-fe:1.0.0 .
    ```
    ![FE](/images/5-frontend/5.1-ec2/011-fe.png?width=90pc)

8. Chạy Docker container
    ```bash
    sudo docker run --name online-shop-frontend --security-opt="no-new-privileges=true" --restart=always -p 3000:3000 -dt online-shop-fe:1.0.0
    ```
    ![FE](/images/5-frontend/5.1-ec2/012-fe.png?width=90pc)