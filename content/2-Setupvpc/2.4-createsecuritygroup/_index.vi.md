---
title : "Tạo Security Groups"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Tạo Security Groups
Trước khi bước vào các bước chính, ta sẽ phân tích việc giao tiếp của các thành phần trong ứng dụng của chúng ta

![Architecture](/images/main_arch.png)

Sau đây là luồng giao tiếp của các thành phần:
- Người dùng sẽ truy cập vào phần giao diện của ứng dụng thông qua DNS của Load Balancer thứ nhất (**Mũi tên xanh**)
- Load Balancer thứ nhất phân bố tải cho các phiên bản của phần giao diện ở 2 vùng khả dụng (**Mũi tên đỏ**)
- Phần giao diện giao tiếp với backend thông qua Load Balancer thứ hai và Load Balancer này phân bố tải cho các phiên bản backend ở 2 vùng khả dụng (**Mũi tên đỏ**)
- Backend sẽ đọc và ghi data vào cơ sở dữ liệu (**Mũi tên đen**)

Tiếp theo, ta sẽ tiến hành khởi tạo các Security Groups dựa vào luồng giao tiếp này

#### Tạo Security Group cho Frontend Load Balancer

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Frontend-LB-SG```
    + Chọn **workshop-1-vpc** VPC
    + Thêm Inbound rules: **HTTP** cho Anywhere IPv4
    + Chọn **Create Security group**

![VPC](/images/2-createvpc/019-createsg.png?width=90pc)
![VPC](/images/2-createvpc/020-createsg.png?width=90pc)

#### Tạo Security Group cho phần Frontend

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Frontend-SG```
    + Chọn **workshop-1-vpc** VPC
    + Thêm Inbound rules:
        - Custom TCP ở port 3000 (Phần Frontend của ứng dụng dùng cổng 3000)
          - Source: **Frontend-LB-SG**
    + Chọn **Create Security group**

![VPC](/images/2-createvpc/021-createsg.png?width=90pc)
![VPC](/images/2-createvpc/022-createsg.png?width=90pc)

#### Tạo Security Group cho Backend Load Balancer

{{% notice note %}}
Bởi vì phần backend có triển khai 1 website để tương tác với API sử dụng Swagger UI tool, chúng ta sẽ thêm 1 rule để cho phép truy cập từ local IP để có thể kiểm tra kết nối giữa backend và cơ sở dữ liệu.
{{% /notice %}}

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Backend-LB-SG```
    + Chọn **workshop-1-vpc** VPC
    + Thêm Inbound rules:
        - HTTP port 80
          - Source: **My IP**
        - HTTP port 80
          - Source: **Frontend-SG**
    + Chọn **Create Security group**

![VPC](/images/2-createvpc/023-createsg.png?width=90pc)
![VPC](/images/2-createvpc/024-createsg.png?width=90pc)

#### Tạo Security Group cho phần Backend của ứng dụng

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Backend-SG```
    + Chọn **workshop-1-vpc** VPC
    + Thêm Inbound rules:
        - Custom TCP ở port 5214 (Phần Backend của ứng dụng sử dụng cổng 5214)
          - Source: **Backend-LB-SG**
    + Chọn **Create Security group**

![VPC](/images/2-createvpc/025-createsg.png?width=90pc)
![VPC](/images/2-createvpc/026-createsg.png?width=90pc)

#### Tạo Security Group cho Cơ sở dữ liệu

{{% notice note %}}
Ở bước trước, ta đã cấu hình bảng định tuyến để cho phép mạng con của cơ sở dữ liệu giao tiếp với Internet. Ở bước này, ta sẽ thêm rule vào Security Group để giới hạn phạm vi chỉ trong local IP của chúng ta. Sau khi hoàn thành việc khởi tạo cơ sở dữ liệu, ta sẽ loại bỏ rule này và ngắt kết nối đến Internet trong bảng định tuyến của cơ sở dữ liệu.
{{% /notice %}}

1. Tại trang chính của VPC
    + Chọn **Security Groups** ở mục Security bên trái.
    + Chọn **Create security group**.

![VPC](/images/2-createvpc/018-createsg.png?width=90pc)

2. Điền thông tin sau
    + Security group name: ```Database-SG```
    + Chọn **workshop-1-vpc** VPC
    + Thêm Inbound rules:
        - MSSQL ở port 1433
          - Source: **Backend-SG**
    + Chọn **Create Security group**

![VPC](/images/2-createvpc/027-createsg.png?width=90pc)
![VPC](/images/2-createvpc/028-createsg-1.png?width=90pc)
![VPC](/images/2-createvpc/028-createsg.png?width=90pc)