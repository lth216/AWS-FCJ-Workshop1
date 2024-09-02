---
title : "Tạo private subnet cho cơ sở dữ liệu"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

#### Tạo private subnet cho Cơ sở dữ liệu
{{% notice note %}}
Dịch vụ AWS RDS yêu cầu các private subnet trong ít nhất 2 vùng khả dụng trong một khu vực AWS cụ thể để đảm bảo tính sẵn có cao. Do đó chúng ta sẽ phải tạo thêm 2 private subnet cho phiên bản cơ sở dữ liệu.
{{% /notice %}}

1. Tại trang chính của VPC Console:
    + Chọn **Subnets** ở thanh công cụ bên trái.
    + Chọn **Create subnet**.

![VPC](/images/2-createvpc/007-createsubnet.png?width=90pc)

{{% notice note %}}
Khi khởi tạo VPC ở bước trước, AWS đã tạo ra 6 private subnet (bao gồm cả public và private) với khối CIDR /20. Theo các private subnet đã được phân bổ, chúng ta có thể thấy rằng có 2 dải địa chỉ chưa được phân bổ là: **10.26.32.0 - 10.26.127.255** và **10.26.192.0 - 10.26.255.255**. Do đó, chúng ta sẽ chọn 2 dải CIDR là **10.26.192.0/20** và **10.26.208.0/20** cho các private subnet bổ sung này, đảm bảo rằng chúng không trùng lắp với các mạng hiện có.
{{% /notice %}}

2. Điền các thông tin sau cho private subnet thứ nhất:
    + Ở mục VPC ID: chọn **workshop-1-vpc** VPC.
    + Thông tin cài đặt cho private subnet:
        - Subnet name: ```private-subnet-rds-1```.
        - Availability Zone: **us-east-1a**.
        - IPv4 subnet CIDR block: ```10.26.192.0/20```.
    + Chọn **Create subnet**

![VPC](/images/2-createvpc/008-createsubnet.png?width=40pc)
![VPC](/images/2-createvpc/009-createsubnet.png?width=40pc)

3. Thực hiện giống bước 1 và 2 để tạo private subnet thứ hai ở vùng khả dụng **us-east-1b** với các thông tin sau: 
    + Subnet name: ```private-subnet-rds-2```
    + Availability Zone: **us-east-1b**
    + IPv4 subnet CIDR block: ```10.26.208.0/20``` (tăng 16 blocks)

![VPC](/images/2-createvpc/010-createsubnet.png?width=90pc)