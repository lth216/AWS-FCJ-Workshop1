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

![Architecture](/images/main_arch.png?width=90pc)

### Nội dung
1. [Giới thiệu](1-Introduction/)
2. [Cài đặt VPC](2-Setupvpc/)
3. [Tạo Database Server](3-CreateRDS/)
4. [Triển khai ứng dụng](4-DeployApp/)
5. [Kiểm tra tính mở rộng](5-TestASG/)
6. [Dọn dẹp tài nguyên](6-Cleanup/)