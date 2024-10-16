---
title : "Kiểm tra khả năng mở rộng của Frontend"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

#### Gỉa lập yêu cầu người dùng với Webserver Stress Tool
1. Tải về và giải nén [Webserver Stress Tool 8](https://www.paessler.com/tools/webstress)

2. Kiểm tra tính mở rộng của Frontend bằng cách cấu hình các thông số sau
    + Test type: CLICKS
      + Run until: **100000** Clicks Per User
    + User simulation
      + Number Of Users: **101**
      + Click Delay: **1**

    ![TEST](/images/7-test/7.3-test/001-testasg.png?width=50pc)

3. Copy tên miền của Application Load Balancer của Frontend và dán vào cột URL ở phần **http://**. Sau đó, nhấp chọn **Start Test**.

    ![TEST](/images/7-test/7.3-test/002-testasg.png?width=50pc)

4. Để tool chạy trong vài phút

    ![TEST](/images/7-test/7.3-test/003-testasg.png?width=50pc)

5. Sau khi tool đã chạy xong, kiểm tra độ mở rộng của EC2 Instance của Frontend thông qua lịch sử hoạt động ở Auto Scaling. Ta có thể thấy rằng số lượng EC2 Instance đã tăng lên là 3

    ![TEST](/images/7-test/7.3-test/004-testasg.png?width=50pc)

6. Kiểm tra Application Load Balancer của Frontend, ta thấy rằng tất cả đều **Healthy**

    ![TEST](/images/7-test/7.3-test/005-testasg.png?width=90pc)

7. Luồng truy cập vào website cũng không bị gián đoạn

    ![TEST](/images/7-test/7.3-test/006-testasg.png?width=90pc)

8. Kiểm tra kết quả

    **Chúng ta có thể thấy rằng mặc dù số lượng yêu cầu truy cập ứng dụng tăng theo thời gian nhưng vẫn không có lỗi xảy ra.**
    ![TEST](/images/7-test/7.3-test/007-testasg.png?width=90pc)

    ![TEST](/images/7-test/7.3-test/008-testasg.png?width=50pc)