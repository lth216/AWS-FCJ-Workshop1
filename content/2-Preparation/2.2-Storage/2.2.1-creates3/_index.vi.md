---
title : "Tạo S3 Bucket"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.2.1 </b> "
---

#### Tạo S3 Bucket ở vùng us-east-1
1. Tại trang chính của AWS
    + Gõ ```S3``` vào thanh tìm kiếm
    + Chọn dịch vụ AWS S3
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/001-s3.png?width=90pc)

2. Tại trang chính của S3
    + Chọn **Buckets** ở menu bên tay trái
    + Chọn **Create bucket**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/002-s3.png?width=90pc)

3. Điền thông tin cho S3 Bucket
{{% notice note %}}
Tên của S3 Bucket không được trùng với bất kì S3 Bucket nào khác trong toàn bộ các tài khoản AWS trong 1 partition. Partition là 1 nhóm các Vùng (Region). Hiện tại, AWS có 3 partitions: aws (Standard Regions), aws-cn (China Regions), and aws-us-gov (AWS GovCloud (US)).\
Tham khảo: [Bucket naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html)
{{% /notice %}}
   + Bucket name: ```my-online-shop-bucket``` (Thay đổi tên khác trong trường hợp của bạn)
   + Để default cho những mục khác
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/003-s3.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/004-s3.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/005-s3.png?width=90pc)

4. Kết quả
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/006-s3.png?width=90pc)

5. Lưu bucket ARN để dùng ở bước sau
    + Chọn **my-online-shop-bucket** bucket
    + Chọn mục **Properties** và copy ARN của bucket
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/007-s3.png?width=90pc)

6. Tải 2 files đính kèm dưới đây. Những file này sẽ được dùng để khởi tạo Database ở các bước sau

    + <a href="/docs/data-init.sql" download>data-init.sql</a>
    + <a href="/docs/table-init.sql" download>table-init.sql</a>

7. Đưa file lên S3 bucket
    + Chọn **my-online-shop-bucket** bucket ở trang chủ S3
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/008-s3.png?width=90pc)

    + Chọn **Upload**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/009-s3.png?width=90pc)

    + Chọn vào **Add files** và chọn 2 file mà ta đã download ở trên. Sau đó, chọn **Upload**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/010-s3.png?width=90pc)

    + Đợi vài giây để tải file lên và sau đó chọn **Close**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/011-s3.png?width=90pc)

8. Kết quả
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/012-s3.png?width=90pc)