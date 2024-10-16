---
title : "Mở rộng ứng dụng với dịch vụ Auto-Scaling Group"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

#### Overview
Auto-Scaling group chứa một tập hợp các Instances EC2 được coi là nhóm hợp lý cho mục đích tự động điều chỉnh và quản lý.

Nó bắt đầu bằng cách khởi chạy đủ số lượng instances để đáp ứng nhu cầu mong muốn. Nó duy trì các instances này bằng cách thực hiện kiểm tra sức khỏe định kỳ trên các instances trong nhóm mục tiêu. Nhóm tự động điều chỉnh tiếp tục duy trì một số lượng instances cố định ngay cả khi một instance trở nên không khỏe bằng cách thay thế nó bằng một instance khác.

Số lượng EC2 Instance được tự động mở rộng/phóng to/thu nhỏ phụ thuộc vào các chính sách tự động mở rộng mà chúng ta định nghĩa cho Auto-Scaling group. 
![ASG](/images/7-test/001-asg.png?width=40pc)

#### Chính sách theo dõi mục tiêu của Auto Scaling group
chính sách theo dõi mục tiêu tự động điều chỉnh khả năng của Auto Scaling group dựa trên giá trị số liệu mục tiêu, mang lại hiệu suất tối ưu và hiệu quả chi phí mà không cần can thiệp thủ công.

Có thể kết hợp nhiều chính sách theo dõi mục tiêu với các số liệu khác nhau để tối ưu hiệu suất mở rộng. Mục đích của nhóm tự động mở rộng luôn là ưu tiên tính sẵn có (High availability).

Có 2 loại chính sách theo dõi mục tiêu trong nhóm tự động điều chỉnh: thông số định sẵn và thông số tùy chỉnh:
1.  Thông số định sẵn
    + **CPU Utilization**: Điều chỉnh số lượng các máy EC2 để duy trì tỷ lệ sử dụng CPU trung bình đã chỉ định trong toàn bộ Auto Scaling group
    + **Request Count per Target**: Theo dõi số lượng yêu cầu được định tuyến đến máy chủ thông qua Application Load Balancer và điều chỉnh quy mô dựa trên số đó. Điều này đảm bảo rằng một máy chủ duy nhất chỉ có thể xử lý một lượng giao thông cụ thể.
    + **Average Network In/Out**: Điều chỉnh số lượng các máy EC2 dựa trên lượng lưu lượng mạng trung bình (vào và ra) trên tất cả các máy chủ. 

2. Thông số tùy chỉnh
    + **Cloudwatch metrics**: Bạn có thể chọn các chỉ số Cloudwatch khác có sẵn hoặc chỉ số tùy chỉnh của riêng bạn trong Cloudwatch. Bạn phải sử dụng AWS CLI hoặc SDK để tạo chính sách theo dõi mục tiêu tùy chỉnh với thông số tùy chỉnh của bạn.

#### Chọn chính sách theo dõi mục tiêu phù hợp cho Frontend và Backend
Ứng dụng backend API bao gồm các tác vụ tốn nhiều CPU như xử lý yêu cầu, truy vấn cơ sở dữ liệu, v.v., dẫn đến tăng sử dụng CPU. Do đó, chính sách **Sử dụng CPU (CPU Utilization)** là lựa chọn hợp lý cho ứng dụng backend.

Ứng dụng frontend thường xử lý tương tác người dùng, phục vụ nội dung tĩnh, thực hiện cuộc gọi đến ứng dụng API. **Số lượng Yêu cầu mỗi Mục tiêu (Request Count per Target)** sẽ đặc biệt hiệu quả khi số lượng yêu cầu HTTP có thể thay đổi dựa trên hoạt động của người dùng, đảm bảo rằng các yêu cầu HTTP/HTTPS giữa các phiên bản được cân bằng.

#### Nội dung
- [Tạo Auto Scaling group cho Backend](7.1-asgbe/)
- [Tạo Auto Scaling group cho Frontend](7.2-asgfe/)
- [Kiểm tra tính mở rộng của Frontend](7.3-test/)