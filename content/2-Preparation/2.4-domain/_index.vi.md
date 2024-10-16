---
title : "Chuẩn bị tên miền với dịch vụ AWS Route 53"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

Ta sẽ dùng các tên miền dễ nhớ để truy cập vào ứng dụng thay vì sử dụng tên miền của CloudFront hay Application Load Balancer. Khi sử dụng tên miền trên AWS với Route 53, ta sẽ có 2 trường hợp:
- Sử dụng Route 53 như là một dịch vụ đăng ký tên miền và định tuyến luồng truy cập đến tên miền.
- Đăng ký tên miền với bên thứ 3 và sử dụng Route 53 như dịch vụ định tuyến luồng truy cập từ tên miền đến AWS Route 53 Hosted Zone.

Đối với bài workshop này, do mình đã có 1 tên miền đăng ký với Hostinger nên mình sẽ sử dụng Route 53 như là dịch vụ định tuyến luồng truy cập.

Để đăng ký tên miền với Route 53, ta có thể tham khảo ở đây: [Register domain with Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html)

#### Tạo Hostez Zone với Route 53

{{% notice note %}}
Route 53 Hosted Zone (DNS Name Server) là 1 kho chứa DNS Record - DNS Record được dùng để định tuyến luồng truy
cập đến tên miền hoặc tên miền phụ. Với mỗi 25 Hosted Zone đầu tiên thì chúng ta sẽ bị tính phí là $0.5 đô mỗi zone mỗi tháng.\
Tham khảo: [AWS Route 53 Pricing](https://aws.amazon.com/route53/pricing/)
{{% /notice %}}
1. Tạo 1 Public Hosted Zone ở Route 53
    + Tại [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#)
      + Gõ ```Route 53``` vào thanh tìm kiếm
      + Chọn dịch vụ AWS Route 53
      ![Route53](/images/2-preparation/2.4-domain/001-route53.png?width=90pc)

2. Chọn **Hosted zones** tại thanh công cụ bên trái và chọn **Create hosted zone**
    ![Route53](/images/2-preparation/2.4-domain/002-route53.png?width=90pc)

3. Cấu hình thông tin cho Hosted Zone
    + Domain name: ```longtruong-lab.online```
    + Description (tùy chọn): ```Hosted zone used for longtruong-lab.online DNS domain```
    + Type: Public hosted zone
    + Nhấp chọn **Create hosted zone**
    ![Route53](/images/2-preparation/2.4-domain/003-route53.png?width=90pc)

4. Bây giờ, ta đã có 1 Hosted Zone cho tên miền **longtruong-lab.online**. AWS tự động tạo NS Record dùng để định tuyến luồng truy cập tới Route 53 Hosted Zone.
    ![Route53](/images/2-preparation/2.4-domain/004-route53.png?width=90pc)

#### Cấu hình Route 53 thành dịch vụ quản lý tên miền cho dịch vụ đăng ký tên miền của bên thứ ba
{{% notice note %}}
Khi cập nhật Name Servers của Route 53 cho miền DNS tại Hostinger, Hostinger sẽ thông báo thay đổi này cho đăng ký miền cấp cao nhất (TLD). Do đó, khi Local DNS Resolver gửi các truy vấn DNS đến đăng ký TLD, nó sẽ kiểm tra cơ sở dữ liệu của mình và phản hồi với các máy chủ tên Route 53, sau đó Local DNS Resolver gửi các truy vấn đến Route 53 và nhận được bản ghi DNS phù hợp, cho phép resolver gửi địa chỉ IP chính xác trở lại trình duyệt.
{{% /notice %}}

1. Truy cập vào [Hostinger domains dashboard](https://hpanel.hostinger.com/domains) và chọn vào tên miền đã đăng ký 
    ![Route53](/images/2-preparation/2.4-domain/005-route53.png?width=90pc)

2. Tại **DNS/Nameservers**, nhấp chọn **Edit**
    ![Route53](/images/2-preparation/2.4-domain/006-route53.png?width=90pc)

3. Tại **DNS records**
    + Nhấp chọn **Change the nameservers**
    + Chọn **Change the nameservers**
    + Copy các nameservers ở NS Record Route 53 Public Hosted Zone và dán vào đây 
    + Nhấp chọn **Rescue**
    ![Route53](/images/2-preparation/2.4-domain/007-route53.png?width=90pc)

4. Nó có thể mất tới 24h để chuyển sang nameserver mới
    ![Route53](/images/2-preparation/2.4-domain/008-route53.png?width=90pc)

5. Để kiểm tra nameserver đã được cập nhật, ta có thể sử dụng câu lệnh **dig** như sau:
    ```bash
    dig longtruong-lab.online NS
    ```
    ![Route53](/images/2-preparation/2.4-domain/009-route53.png?width=40pc)