---
title : "Triển khai ứng dụng web đa tầng bằng dịch vụ của AWS"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---
# Triển khai ứng dụng web đa tầng bằng dịch vụ của AWS

### Tổng quan
Ở workshop này, chúng ta sẽ tiến hành triển khai một ứng dụng web đa tầng (frontend, backend và cơ sở dữ liệu) dưới dạng Docker container sử dụng các dịch vụ của AWS, đảm bảo tính mở rộng và bảo mật cho ứng dụng.

Đây là tổng quan về các dịch vụ AWS mà ta sẽ cấu hình và sử dụng:

![Architecture](/images/additional/main_arch.png?width=50pc)

### Content
1. [Giới thiệu](1-Introduction/)
2. [Chuẩn bị môi trường](2-Preparation/)
3. [Triển khai Database server](3-DeployDatabase/)
4. [Triển khai phần Backend](4-DeployBE/)
5. [Triển khai phần Frontend](5-DeployFE/)
6. [Tối ưu độ trễ với CloudFront Distribution](6-Connectivity/)
7. [Khả năng mở rộng với Auto-Scaling group](7-Scalability/)
8. [Dọn dẹp tài nguyên](8-Cleanup/)