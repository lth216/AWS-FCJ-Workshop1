---
title : "Create S3 Bucket"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.2.1 </b> "
---

#### Create S3 Bucket in us-east-1 region
1. In the AWS Management Console
    + Type ```S3``` in the search box
    + Choose **AWS S3** service
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/001-s3.png?width=90pc)

2. In the AWS S3 Management Console
    + Choose **Buckets** in the left sidebar
    + Choose **Create bucket**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/002-s3.png?width=90pc)

3. Specify information for the S3 Bucket
{{% notice note %}}
S3 Bucket name must be unique accross all AWS accounts in all AWS Regions within a partition. A partition is a group of Regions. AWS currently has 3 partitions: aws (Standard Regions), aws-cn (China Regions), and aws-us-gov (AWS GovCloud (US)).\
Reference: [Bucket naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html)
{{% /notice %}}
   + Bucket name: ```my-online-shop-bucket``` (Choose another unique name for your case)
   + Keep other options as default
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/003-s3.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/004-s3.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/005-s3.png?width=90pc)

4. Result
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/006-s3.png?width=90pc)

5. Copy the bucket ARN for latter usage
    + Click to the bucket name
    + Choose **Properties** section and copy the ARN of bucket
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/007-s3.png?width=90pc)

6. Download 2 files in the attachment below. These files contain SQL queries for initializing database

    + <a href="/docs/data-init.sql" download>data-init.sql</a>
    + <a href="/docs/table-init.sql" download>table-init.sql</a>

7. Upload files to S3 Bucket
    + Click to the **my-online-shop-bucket** in the S3 Management Console
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/008-s3.png?width=90pc)

    + Choose **Upload**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/009-s3.png?width=90pc)

    + Click to **Add files** and choose 2 files that we have already downloaded. Then, click **Upload**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/010-s3.png?width=90pc)

    + Wait a few seconds to upload those files, and then click **Close**
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/011-s3.png?width=90pc)

8. Result
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/012-s3.png?width=90pc)