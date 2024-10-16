---
title : "Giới thiệu"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

#### Giới thiệu
Trong workshop này, chúng ta sẽ khám phá cách thiết lập một môi trường lưu trữ web có khả năng mở rộng, bền vững và an toàn trên AWS bằng cách tận dụng sự kết hợp của các dịch vụ AWS. Dưới đây là một giới thiệu ngắn gọn về các dịch vụ AWS cốt lõi chúng ta sẽ sử dụng.

1. Networking
    
    **AWS VPC (Virtual Private Cloud)** cho phép bạn tạo ra một mạng cách ly logic trong đám mây AWS. Dịch vụ này cho phép bạn kiểm soát môi trường mạng ảo của mình, bao gồm các dải địa chỉ IP, các mạng con, bảng định tuyến và cài đặt bảo mật. Chúng ta sẽ sử dụng VPC để xác định kiến trúc mạng và bảo vệ tài nguyên của chúng ta.

    **Amazon Route 53** là một dịch vụ web Hệ thống Tên miền (DNS) có khả năng sẵn có và mở rộng. Bạn có thể sử dụng Route 53 để thực hiện ba chức năng chính theo bất kỳ sự kết hợp nào: đăng ký tên miền, định tuyến DNS và kiểm tra sức khỏe web.

    **AWS Certificate Manager** quản lý sự phức tạp của việc tạo, lưu trữ và gia hạn chứng chỉ và khóa SSL/TLS X.509 công cộng và riêng tư bảo vệ trang web và ứng dụng AWS của bạn. Bạn có thể cung cấp chứng chỉ cho các dịch vụ AWS tích hợp của bạn bằng cách phát hành chúng trực tiếp với ACM hoặc bằng cách nhập chứng chỉ của bên thứ ba vào hệ thống quản lý ACM. Chứng chỉ ACM có thể bảo mật tên miền đơn, nhiều tên miền cụ thể, tên miền đại diện hoặc sự kết hợp của chúng. Chứng chỉ ACM với ký tự đại diện có thể bảo vệ một số lượng không giới hạn các tên miền con. Bạn cũng có thể xuất chứng chỉ ACM được ký bởi AWS Private CA để sử dụng ở bất kỳ đâu trong hệ thống PKI nội bộ của bạn..

2. Compute
    
    **AWS EC2 (Elastic Compute Cloud)** cung cấp các máy chủ ảo có khả năng mở rộng trong đám mây, được gọi là các phiên bản. Các phiên bản này cho phép bạn chạy ứng dụng trong một môi trường tính toán an toàn và có thể điều chỉnh kích thước. Trong workshop này, chúng ta sẽ sử dụng các phiên bản EC2 để lưu trữ máy chủ web của chúng ta, bao gồm phần giao diện và phần backend.

3. Database
    
    **Amazon RDS (Relational Database Service)** là một dịch vụ quản lý cơ sở dữ liệu hỗ trợ nhiều hệ quản trị cơ sở dữ liệu, bao gồm MySQL, Microsoft SQL, PostgreSQL và Oracle. RDS tự động hóa các nhiệm vụ quản trị tốn thời gian như sao lưu, quản lý bản vá và mở rộng. Trong cấu hình của chúng ta, RDS sẽ được sử dụng để lưu trữ cơ sở dữ liệu phía sau cho ứng dụng web.

4. Scalability
    
    **AWS ALB (Application Load Balancer)** phân phối lưu lượng đến nhiều mục tiêu, chẳng hạn như các phiên bản EC2, trong một hoặc nhiều Khu vực khả dụng. ALB hoạt động ở tầng ứng dụng (Tầng 7) và cung cấp khả năng định tuyến tiên tiến, đảm bảo tính sẵn có cao và khả năng chịu lỗi cho ứng dụng web của bạn.

    **Amazon Auto Scaling Group** tự động điều chỉnh số lượng các phiên bản EC2 để đáp ứng nhu cầu của ứng dụng của bạn. Nó giúp duy trì hiệu suất tối ưu bằng cách thêm hoặc loại bỏ các phiên bản dựa trên mô hình lưu lượng. Workshop này sẽ hướng dẫn cách thiết lập một ASG để đảm bảo ứng dụng web của bạn có thể tự động mở rộng dựa trên các yêu cầu từ người dùng.

5. Latency
    
    **Amazon CloudFront** là một dịch vụ web giúp tăng tốc việc phân phối nội dung web tĩnh và động của bạn, như các tệp .html, .css, .js và hình ảnh, đến người dùng của bạn. CloudFront cung cấp nội dung của bạn thông qua một mạng lưới toàn cầu các trung tâm dữ liệu gọi là edge location. Khi người dùng yêu cầu nội dung mà bạn đang phục vụ bằng CloudFront, yêu cầu sẽ được định tuyến đến edge location cung cấp độ trễ thấp nhất (thời gian chờ), để nội dung được giao tới với hiệu suất tốt nhất.

    - Nếu nội dung đã có sẵn trong vị trí cạnh có độ trễ thấp nhất, CloudFront sẽ giao nó ngay lập tức.

    - Nếu nội dung không nằm trong vị trí cạnh đó, CloudFront sẽ lấy nó từ nguồn mà bạn đã xác định - chẳng hạn như một bucket Amazon S3, một kênh MediaPackage hoặc một máy chủ HTTP (ví dụ: máy chủ web) mà bạn đã xác định là nguồn cho phiên bản xác định của nội dung của bạn.

**Cùng thực hành nào !**