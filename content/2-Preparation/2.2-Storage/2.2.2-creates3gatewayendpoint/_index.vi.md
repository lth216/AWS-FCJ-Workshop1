---
title : "Tạo S3 Gateway Endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

#### Create S3 Gateway Endpoint
1. Tại trang chính của AWS
    + Gõ ```VPC``` vào thanh tìm kiếm
    + Chọn dịch vụ AWS VPC
    ![S3](/images/2-preparation/2.1-networking/2.1-vpc/001-vpc.png?width=90pc)

2. Tạo VPC endpoint
    + Chọn **Endpoints** ở menu bên trái
    + Nhấp chọn **Create endpoint**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/001-s3gw.png?width=90pc)

3. Cấu hình VPC endpoint như sau
    + Name tag: ```workshop1-s3-gateway-endpoint```
    + Chọn **AWS services** ở mục Service category
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/002-s3gw.png?width=90pc)

    + Gõ ```s3``` vào thanh tìm kiếm và chọn Service S3 với **Gateway** type
    + Chọn VPC: **workshop-1-vpc**
    + Route tables: Tick chọn vào các route tables của các private subnets
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/003-s3gw.png?width=90pc)

    + Chọn **Custom** policy cho VPC endpoint policy.
    + Copy and dán nội dung sau vào editor của endpoint policy
{{% notice note %}}
Ở các bước sau, ta sẽ tạo Docker image để triển khai ứng dụng trong container với Docker và các Docker image này sử dụng base image được tải từ Docker Hub. Docker Hub lưu trữ tài nguyên của nó trong AWS S3 Bucket, vì vậy khi chúng ta gửi request đến Docker Hub để tải base image về thì các request sẽ không đi qua NAT Gateway mà đi qua S3 Gateway Endpoint do AWS phát hiện được S3 Bucket trong nameserver của Docker Hub.\
Để kiểm tra nameserver của Docker Hub, ta có thể sử dụng câu lệnh **dig** như sau: ```$ dig docker.io NS```
{{% /notice %}}

    + Copy and paste nội dung sau vào Editor của VPC endpoint. Policy này cho phép các kết nối tới S3 Bucket thông qua VPC endpoint download, upload and liệt kê các objects trong tất cả S3 Bucket ở AWS. Sau đó, nhấp chọn **Create endpoint**.
    
        ```JSON
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "AccessToS3Bucket",
                    "Effect": "Allow",
                    "Principal": "*",
                    "Action": [
                        "s3:GetObject",
                        "s3:PutObject",
                        "s3:List*"
                    ],
                    "Resource": "*"
                }
            ]
        }
        ```

4. Kiểm tra kết quả
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/005-s3gw.png?width=90pc)

5. Copy và lưu VPC Endpoint ID
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/006-s3gw.png?width=90pc)

6. AWS sẽ tự động thêm 1 định tuyến đến VPC endpoint ở các Route table mà ta đã add vào khi tạo VPC endpoint
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/007-s3gw.png?width=90pc)

#### Tạo bucket policy cho  S3 bucket
Ở bước này, ta sẽ tạo 1 bucket policy để cho phép các hành động: tải, gửi lên và liệt kê các object ở bucket my-online-shop-bucket từ các kết nối thông qua VPC endpoint ID được tạo ở trên.

1. Nhấp chọn **my-online-shop-bucket** ở trang chủ S3
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/008-s3.png?width=90pc)

2. Chọn mục **Permissions** và chọn **Edit** ở mục Bucket policy section
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/008-s3gw.png?width=90pc)

3. Copy and dán nội dung sau vào editor của bucket policy và sau đó chọn **Save changes**
    ```JSON
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AllowAccessFromVPCEndpoint",
                "Effect": "Allow",
                "Principal": "*",
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:List*"
                ],
                "Resource": [
                    "arn:aws:s3:::my-online-shop-bucket",
                    "arn:aws:s3:::my-online-shop-bucket/*"
                ],
                "Condition": {
                    "StringLike": {
                        "aws:SourceVpce": "vpce-0ff31b2e01c38ff89"
                    }
                }
            }
        ]
    }
    ```
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/009-s3gw.png?width=90pc)

#### Create IAM Role for EC2 Instance to interact with S3 Bucket
{{% notice note %}}
Ở các bước sau, ta sẽ dùng AWS CLI để copy files từ S3 Bucket, và việc này cần xác minh và xác thực AWS Account. Cách dễ nhất chính là tạo ACCESS_KEY và SECRET_ACCESS_KEY và lưu nó ở setting của AWS CLI bằng cách sử dụng câu lệnh **aws configure**. Tuy nhiên, cách này không an toàn vì nó yêu cầu chúng ta phải lưu key quan trọng trên máy chủ. Thay vào đó, ta sẽ tạo 1 IAM Role và gán vào EC2 Instance để từ đó ta có thể sử dụng AWS CLI mà không cần access key.
{{% /notice %}}

1. Ở trang chủ IAM [IAM Management Console](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home)
    + Chọn **Roles**
    + Nhấp chọn **Create role**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/010-s3gw.png?width=90pc)

2. Chọn **EC2 Instance** ở mục Trusted Entity để EC2 Instance có thể sử dụng các quyền ở role này
    + Trusted entity type: **AWS service**
    + Use case: **EC2**
    + Nhấp chọn **Next**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/011-s3gw.png?width=90pc)

3. Tìm ```AmazonS3ReadOnlyAccess``` policy và chọn nó. Sau đó, nhấp chọn **Next**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/012-s3gw.png?width=90pc)

4. Đặt tên, review và tạo IAM Role
    + Role name: ```AllowEC2ReadS3```
    + Description: ```Allows EC2 instances to read S3 Bucket.```
    + Sử dụng default cho các lựa chọn khác
    + Nhấp chọn **Create role**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/013-s3gw.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/014-s3gw.png?width=90pc)

5. Kiểm tra rằng IAM Role đã được tạo thành công
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/015-s3gw.png?width=90pc)