---
title : "Giới thiệu"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

#### Giới thiệu
Trong workshop này, chúng ta sẽ khám phá cách thiết lập một môi trường lưu trữ web có khả năng mở rộng, bền vững và an toàn trên AWS bằng cách tận dụng sự kết hợp của các dịch vụ AWS. Dưới đây là một giới thiệu ngắn gọn về các dịch vụ AWS cốt lõi chúng ta sẽ sử dụng.

**AWS VPC (Virtual Private Cloud)** cho phép bạn tạo ra một mạng cách ly logic trong đám mây AWS. Dịch vụ này cho phép bạn kiểm soát môi trường mạng ảo của mình, bao gồm các dải địa chỉ IP, các mạng con, bảng định tuyến và cài đặt bảo mật. Chúng ta sẽ sử dụng VPC để xác định kiến trúc mạng và bảo vệ tài nguyên của chúng ta.

**AWS EC2 (Elastic Compute Cloud)** cung cấp các máy chủ ảo có khả năng mở rộng trong đám mây, được gọi là các phiên bản. Các phiên bản này cho phép bạn chạy ứng dụng trong một môi trường tính toán an toàn và có thể điều chỉnh kích thước. Trong workshop này, chúng ta sẽ sử dụng các phiên bản EC2 để lưu trữ máy chủ web của chúng ta, bao gồm phần giao diện và phần backend.

**AWS ALB (Application Load Balancer)** phân phối lưu lượng đến nhiều mục tiêu, chẳng hạn như các phiên bản EC2, trong một hoặc nhiều Khu vực khả dụng. ALB hoạt động ở tầng ứng dụng (Tầng 7) và cung cấp khả năng định tuyến tiên tiến, đảm bảo tính sẵn có cao và khả năng chịu lỗi cho ứng dụng web của bạn.

**Amazon Auto Scaling Group** tự động điều chỉnh số lượng các phiên bản EC2 để đáp ứng nhu cầu của ứng dụng của bạn. Nó giúp duy trì hiệu suất tối ưu bằng cách thêm hoặc loại bỏ các phiên bản dựa trên mô hình lưu lượng. Workshop này sẽ hướng dẫn cách thiết lập một ASG để đảm bảo ứng dụng web của bạn có thể tự động mở rộng dựa trên các yêu cầu từ người dùng.

**Amazon RDS (Relational Database Service)** là một dịch vụ quản lý cơ sở dữ liệu hỗ trợ nhiều hệ quản trị cơ sở dữ liệu, bao gồm MySQL, Microsoft SQL, PostgreSQL và Oracle. RDS tự động hóa các nhiệm vụ quản trị tốn thời gian như sao lưu, quản lý bản vá và mở rộng. Trong cấu hình của chúng ta, RDS sẽ được sử dụng để lưu trữ cơ sở dữ liệu phía sau cho ứng dụng web.

**Cùng thực hành nào !**