---
title : "Tạo VPC tùy chỉnh"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

Trong phần này, chúng ta sẽ tạo một VPC tùy chỉnh và thiết lập bảng định tuyến, nhóm bảo mật cũng như tạo điểm kết nối EC2 Instance Connect để kết nối với các EC2 Instances. Sau đó, chúng ta sẽ triển khai một ứng dụng web Online Shop có khả năng tự động mở rộng trên 2 Khu vực Khả dụng trong trường hợp truy cập yêu cầu cao.

#### Content
- [Tạo VPC](2.1-createvpc/)
- [Tạo Subnets cho Database server](2.2-createrdssubnets/)
- [Cấu hình Route table cho Database](2.3-configureroutetable/)
- [Tạo Security groups](2.4-createsecuritygroup/)
- [Tạo EC2 Instance Connect Endpoint](2.5-createeic/)
