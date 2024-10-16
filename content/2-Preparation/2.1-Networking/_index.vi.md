---
title : "Cấu hình mạng"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

Đối với phần Networking trong bài workshop này, ta sẽ tạo 1 VPC ở region N. Virginia (us-east-1) để triển khai ứng dụng lên đó. Ta sẽ dùng 2 Availability Zones cùng với 8 subnets, trong đó: 2 public subnets, 2 private subnets cho Frontend, 2 private subnets cho Backend và 2 private subnets cho Database server. Bên cạnh đó, ta cũng sẽ cấu hình Security Group cho các thành phần trong ứng dụng của chúng ta để có thể giao tiếp được với nhau.

Hạ tầng sẽ được triển khai như sau:
![Networking](/images/additional/networking.png)

#### Nội dung
- [Tạo VPC](2.1.1-CreateVPC/)
- [Tạo subnet cho Database server](2.1.2-Createsubnet/)
- [Cấu hình Security Groups](2.1.3-Configuresg/)
