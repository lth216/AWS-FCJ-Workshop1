---
title : "Tạo private subnet cho database server"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

#### Tạo private subnet cho Cơ sở dữ liệu
{{% notice tip %}}
Để có thể tạo Database server thì VPC của chúng ta phải có ít nhất 2 subnets nằm ở 2 Availability Zones khác nhau. Thêm vào đó, server Database nên được triển khai ở các subnet riêng biệt với ứng dụng để hạn chế quyền truy cập tới Database. Chính vì thế, ta sẽ tạo thêm 2 private subnets để triển khai Database server.\
Tham khảo thêm tại đây: [Tips working with Database instance in a VPC](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#Overview.RDSVPC.Create)
{{% /notice %}}

1. Tại trang chính của VPC Console:
    + Chọn **Subnets** ở thanh công cụ bên trái.
    + Chọn **Create subnet**.
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/001-subnet.png?width=90pc)

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
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/002-subnet.png?width=90pc)

3. Tạo 1 private subnet khác ở Availability Zone us-east-1b
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/001-subnet.png?width=90pc)

4. Điền thông tin sau: 
    + Subnet name: ```private-subnet-rds-2```
    + Availability Zone: **us-east-1b**
    + IPv4 subnet CIDR block: ```10.26.208.0/20``` (tăng 16 blocks)

    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/003-subnet.png?width=90pc)

5. Kết quả sau khi tạo 2 private subnets cho Database server như sau
    ![VPC](/images/2-preparation/2.1-networking/2.2-subnet/004-subnet.png?width=90pc)