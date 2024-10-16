---
title : "Giảm độ trễ với CloudFront Distribution"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

Trong phần này, chúng ta sẽ triển khai CloudFront để tăng tốc việc phân phối nội dung web tĩnh và động đến người dùng. CloudFront định tuyến yêu cầu của người dùng qua mạng lưới AWS đến vị trí cạnh tốt nhất có thể phục vụ nội dung của chúng ta, giảm số lượng mạng mà yêu cầu của người dùng phải đi qua, và do đó cải thiện hiệu suất của ứng dụng. Vì chiến lược của chúng ta là cải thiện bảo mật của ứng dụng web, chúng ta sẽ cấu hình CloudFront yêu cầu HTTPS cả để giao tiếp với người xem và để giao tiếp với nguồn gốc (trong trường hợp này là Frontend Load Balancer).
Cuối cùng, chúng ta sẽ thay thế tên miền DNS của CloudFront bằng tên miền riêng của chúng ta (được tạo trong bước 2) mà thân thiện và dễ nhớ hơn.

#### Nội dung
- [Đăng ký chứng chỉ SSL với AWS ACM](6.1-createacm/)
- [Tạo CloudFront và kết nối với ALB](6.2-createcloudfront/)