---
title : "Tạo Load Balancer cho Online Shop Backend"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Tạo Target Groups
1. Tại trang chính của EC2, chọn **Target Groups** ở trong mục Load Balancing bên trái và nhấp chọn **Create target group**

![EC2](/images/4-createec2/014-createec2.png?width=90pc)

2. Điền thông tin cho Target Group như bên dưới:
    + Target type: **Instance**
    + Target group name: ```Backend-TG```
    + Protocol: **HTTP port 5214**
    + VPC: **workshop-1-vpc**
    + Health check path: ```/swagger/index.html``` (suffix của API doc website)
    + Nhấp chọn **Next**

![EC2](/images/4-createec2/015-createec2.png?width=40pc)
![EC2](/images/4-createec2/016-createec2.png?width=40pc)
![EC2](/images/4-createec2/017-createec2.png?width=40pc)

3. Gán EC2 Instance của backend vào Target Group
    + Chọn Online-Shop-Backend EC2 Instance ở Register target và nhấp chọn **Include as pending below**.
    + Sau đó chọn **Create target group**

![EC2](/images/4-createec2/018-createec2.png?width=90pc)
![EC2](/images/4-createec2/019-createec2.png?width=90pc)

4. Kiểm tra kết quả

![EC2](/images/4-createec2/020-createec2.png?width=90pc)

#### Tạo Backend Load Balancer
1. Tại trang chính của EC2, chọn **Load Balancers** trong mục Load Balancing và chọn **Create load balancer**

![EC2](/images/4-createec2/021-createec2.png?width=90pc)

2. Chọn Application Load Balancer

![EC2](/images/4-createec2/022-createec2.png?width=40pc)

3. Cấu hình Backend Load Balancer như sau:
    + Load balancer name: ```Online-Shop-BE-LB```
    + Scheme: **Internet-facing**
    + VPC: **workshop-1-vpc**
    + Availability Zones: **workshop-1-subnet-public1-us-east-1a** và **workshop-1-subnet-public2-us-east-1b**
    + Security groups: **Backend-LB-SG**
    + Target group: **Backend-TG**
    + Chọn **Create load balancer**

![EC2](/images/4-createec2/023-createec2.png?width=50pc)
![EC2](/images/4-createec2/024-createec2.png?width=50pc)
![EC2](/images/4-createec2/025-createec2.png?width=50pc)
![EC2](/images/4-createec2/026-createec2.png?width=50pc)

{{% notice note %}}
Để cho đơn giản trong workshop này thì ta sẽ triển khai Load Balancer ở public subnet, cho phép chúng ta truy vấn tới Load Balancer DNS từ browser.
{{% /notice %}}

4. Đợi vài phút để khởi tạo Backend Load Balancer

![EC2](/images/4-createec2/027-createec2.png?width=90pc)

5. Nếu Load Balancer hoạt động bình thường, trạng thái hiển thị sẽ là <span style="color:green">Healthy</span>

![EC2](/images/4-createec2/028-createec2.png?width=90pc)

6. Truy cập tới API doc của backend bằng DNS của Load Balancer từ browser

URL: **<_Load_Balancer_DNS_>/swagger/index.html**

![EC2](/images/4-createec2/029-createec2.png?width=90pc)

7. Kiểm tra kết nối tới CSDL bằng cách tương tác với API doc

Lựa chọn mục GET /api/Product của Product và chọn **Execute**

![EC2](/images/4-createec2/030-createec2.png?width=90pc)
