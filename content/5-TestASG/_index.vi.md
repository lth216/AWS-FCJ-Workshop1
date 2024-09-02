---
title : "Kiểm tra tính mở rộng của ứng dụng Online Shop"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

#### Kiểm tra tính mở rộng của ứng dụng Online Shop
Ở bước này, ta sẽ kiểm tra tính mở rộng của ứng dụng bằng cách gửi nhiều request truy cập đến ứng dụng dùng 1 tool gọi là Webserver Stress Tool 8 để mô phỏng request của nhiều người dùng

1. Tải và giải nén tool [Webserver Stress Tool 8](https://www.paessler.com/tools/webstress)

2. Điền các thông số test sau
    + Test type: CLICKS
      + Run until: **100000** Clicks Per User
    + User simulation
      + Number Of Users: **101**
      + Click Delay: **1**

![TEST](/images/5-testasg/001-testasg.png?width=50pc)

3. Sao chép DNS của **Frontend Load Balancer** và dán vào cột URL trong mục **http://**. Sau đó, nhấp chọn **Start Test**.

![TEST](/images/5-testasg/002-testasg.png?width=50pc)

4. Để tool chạy trong vài phút

![TEST](/images/5-testasg/003-testasg.png?width=50pc)

5. Kiểm tra Activity history của Auto Scaling group của Frontend. Lúc này, ta sẽ thấy số lượng EC2 Instance của Frontend đã tăng lên 3

![TEST](/images/5-testasg/004-testasg.png?width=50pc)

6. Kiểm tra **Frontend Load Balancer**, ta thấy trạng thái Load Balancer của Frontend là **Healthy**, chứng tỏ rằng phần mở rộng của ứng dụng hoạt động bình thường.

![TEST](/images/5-testasg/005-testasg.png?width=90pc)

7. Việc kết nối đến giao diện website cũng không bị gián đoạn

![TEST](/images/5-testasg/006-testasg.png?width=90pc)

8. Đây là một số kết quả sau khi tool chạy test xong.

![TEST](/images/5-testasg/007-testasg.png?width=90pc)

![TEST](/images/5-testasg/008-testasg.png?width=50pc)