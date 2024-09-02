---
title : "Cấu hình bảng định tuyến cho private subnet của cơ sở dữ liệu"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

#### Cấu hình bảng định tuyến cho private subnet của cơ sở dữ liệu

{{% notice note %}}
Thông thường, chúng ta sẽ đặt cơ sở dữ liệu của chúng ta trong một private subnet và chặn kết nối từ Internet để tăng tính bảo mật. Tuy nhiên, đối với lần khởi tạo cơ sở dữ liệu đầu tiên, chúng ta cần tương tác với cơ sở dữ liệu để tạo các bảng và thêm một vài thông tin cần thiết. Để làm cho quá trình này dễ dàng hơn, ta sẽ thiết lập một bảng định tuyến tạm thời cho phép kết nối từ địa chỉ IP cục bộ của chúng ta để có thể dùng các tool hỗ trợ GUI kết nối đến cơ sở dữ liệu. Ta sẽ sử dụng một tool có tên gọi là SQL Server Management Studio (SSMS).
{{% /notice %}}

1. Tại trang chính của VPC
    + Chọn **Route tables** ở menu bên trái.
    + Chọn **Create route table**.

![VPC](/images/2-createvpc/011-routetable.png?width=90pc)

2. Tạo bảng định tuyến với các thông tin sau
    + Route table name: ```workshop-1-rds-rt```
    + VPC: **workshop-1-vpc**
    + Chọn **Create route table**

![VPC](/images/2-createvpc/012-routetable.png?width=40pc)

3. Chỉnh sửa đường đi (route) cho bảng định tuyến
    + Nhấp chọn vào bảng định tuyến vừa tạo
    + Chọn **Edit routes** ở mục Routes

![VPC](/images/2-createvpc/013-routetable.png?width=90pc)

4. Thêm route sau
    + Destination: 0.0.0.0/0
    + Target: ```workshop-1-igw``` (Internet Gateway của workshop-1 VPC) 
    + Sau đó chọn **Save changes**

![VPC](/images/2-createvpc/014-routetable.png?width=90pc)

{{% notice note %}}
Để cho phép các kết nối chỉ từ local IP, ta sẽ cấu hình Security Group ở phần sau.
{{% /notice %}}

5. Gán các private subnet của cơ sở dữ liệu vào bảng định tuyến

- Bấm vào mục **Subnet associations** và chọn **Edit subnet associations**

![VPC](/images/2-createvpc/015-routetable.png?width=90pc)

- Chọn **private-subnet-rds-1** và **private-subnet-rds-2**. Sau đó chọn **Save associations**

![VPC](/images/2-createvpc/016-routetable.png?width=90pc)

6. Kiểm tra kết quả

![VPC](/images/2-createvpc/017-routetable.png?width=90pc)