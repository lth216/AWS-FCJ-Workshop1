---
title : "Tạo chứng chỉ SSL/TLS cho ALB của Frontend"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---
{{% notice note %}}
Chúng ta chỉ cần kích hoạt mã hóa thông tin với SSL/TLS tại mức độ ALB, điều này có nghĩa là ALB sẽ xử lý việc mã hóa và giải mã lưu lượng HTTPS giữa ALB và khách hàng. Vì ứng dụng Backend được triển khai trong private subnet VPC, gây ra ít rủi ro về bảo mật, chúng ta không cần mã hóa lưu lượng giữa ALB và Target Group của Backend. Do đó, chúng ta không cần kích hoạt HTTPS cho Target Group của Backend trừ khi chúng ta bắt buộc phải sử dụng **end-to-end encryption** dựa trên quy định của công ty.
{{% /notice %}}

#### Thêm HTTPS Inbound rule cho Load Balancer security group
1. Tại trang chủ của EC2
    + Chọn **Security Group** ở thanh công cụ bên trái
    + Chọn **Frontend-LB-SG** security group
    + Chọn **Edit inbound rules**
    ![FE](/images/5-frontend/5.3-https/001-https-fe.png?width=90pc)

2.Thêm Inbound rule
    + Type: **HTTPS**
    + Source: **Anywhere-IPv4**
    + Nhấp chọn **Save rules**
    ![FE](/images/5-frontend/5.3-https/002-https-fe.png?width=90pc)

#### #### Đăng kí 1 chứng chỉ SSL/TLS certificate cho tên miển được sử dụng cho ALB của Frontend
1. Tại [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Gõ ```ACM``` vào thanh tìm kiếm
    + Chọn dịch vụ AWS ACM
    ![FE](/images/5-frontend/5.3-https/003-https-fe.png?width=90pc)

2. Tại trang chính của dịch vụ AWS Certificate Manager, nhấp chọn **Request a certificate**
{{% notice note%}}
AWS không tính phí cho chứng chỉ SSL/TLS public.\
Tham khảo tại đây: [AWS Certificate Manager Pricing](https://aws.amazon.com/certificate-manager/pricing/)
{{% /notice %}}
    ![FE](/images/5-frontend/5.3-https/004-https-fe.png?width=90pc)

3. Nhấp chọn **Next** để tạo chứng chỉ SSL/TLS public
    ![FE](/images/5-frontend/5.3-https/005-https-fe.png?width=90pc)

4. Điền các thông tin như bên dưới
    + FQDN: ```ui.longtruong-lab.online``` **(Change the FQDN according to your use case)**
    + Validation method: **DNS validation**
    + Key algorithm: **RSA 2048**
    + Nhấp chọn **Request**
    ![FE](/images/5-frontend/5.3-https/006-https-fe.png?width=90pc)

5. Một chứng chỉ SSL với trạng thái "Pending validation" được tạo
{{% notice note %}}
Với phương pháp xác thực DNS, AWS sẽ tạo một hoặc nhiều bản ghi CNAME tùy thuộc vào FQDN mà chúng ta cung cấp. Chúng ta phải thêm bản ghi này vào Zone Hosted của Route 53 để kích hoạt chứng chỉ và chỉ cần làm điều này một lần duy nhất. Sau đó, trạng thái của chứng chỉ là "Đã cấp". 
{{% /notice %}}
    + Nhấp chọn **Create record in Route 53 button**
    ![FE](/images/5-frontend/5.3-https/007-https-fe.png?width=90pc)

6. Vì Route 53 và ACM của AWS liên kết chặt chẽ với nhau nên ta không cần phải điền thông thông tin gì. Chỉ cần nhấp chọn **Create records**.
    ![FE](/images/5-frontend/5.3-https/008-https-fe.png?width=90pc)

7. Sau một vài phút, trạng thái của chứng chỉ sẽ là "Issued"
    ![FE](/images/5-frontend/5.3-https/009-https-fe.png?width=90pc)

#### Tạo HTTPS Listener and rules cho ALB của Backend
1. Tại trang chính của EC2
    + Chọn **Load Balancers** bên thanh công cụ bên trái
    + Chọn **Online-Shop-FE-LB** load balancer
    + Chuyển sang tùy chọn **Listeners and rules** 
    + Chọn **Add listener**
    ![FE](/images/5-frontend/5.3-https/010-https-fe.png?width=90pc)

2. Ở phần cấu hình của Listener, điền thông tin như sau 
    + Protocol: **HTTPS**
    + Port: **443**
    + Routing actions: Forward to target groups
    + Target group: **Frontend-TG**
    ![FE](/images/5-frontend/5.3-https/011-https-fe.png?width=90pc)

3. Ở vùng cài đặt bảo mật cho Listener, cấu hình như sau:
    + Certificate source: **From ACM**
    + Certificate: **ui.longtruong-lab.online** (Chọn tên miền bạn đã đăng ký ứng với chứng chỉ)
    + Nhấp chọn **Save changes**.
    ![FE](/images/5-frontend/5.3-https/012-https-fe.png?width=90pc)

4. Lúc này ta sẽ có 1 HTTPS Listener cho ALB của Frontend
    ![FE](/images/5-frontend/5.3-https/013-https-fe.png?width=90pc)

5. Truy cập vào Frontend thông qua kết nối HTTPS
    + The URL: https://ui.longtruong-lab.online
    ![FE](/images/5-frontend/5.3-https/014-https-fe.png?width=90pc)