---
title : "Create S3 Gateway Endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

#### Create S3 Gateway Endpoint
1. In the AWS Management Console
    + Type ```VPC``` to the search box
    + Choose **AWS VPC** service
    ![S3](/images/2-preparation/2.1-networking/2.1-vpc/001-vpc.png?width=90pc)

2. Create VPC endpoint
    + Choose **Endpoints** in the left menu
    + Click **Create endpoint**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/001-s3gw.png?width=90pc)

3. Configure VPC endpoint specifications
    + Name tag: ```workshop1-s3-gateway-endpoint```
    + Choose **AWS services** in Service category
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/002-s3gw.png?width=90pc)

    + Type ```s3``` to the search bar and choose S3 service with **Gateway** type
    + Choose VPC: **workshop-1-vpc**
    + Route tables: Check to all route tables of private subnets
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/003-s3gw.png?width=90pc)

    + Choose **Custom** policy for VPC endpoint policy.
{{% notice note %}}
During the operation, we will create Docker image for our application which downloads the base images from Docker Hub. Docker Hub stores its resources in AWS S3 Bucket so that when we build a Docker image within the EC2 Instance inside the private subnet of VPC, the requests won't go to Docker Hub storage by Internet through NAT Gateway. Instead, it will go through VPC Gateway Endpoint that we've already created since AWS detects that Docker Hub storage is S3 Bucket by checking its nameservers.\
To check the nameserver of Docker Hub, we can use **dig** command: ```$ dig docker.io NS```
{{% /notice %}}

    + Copy and paste the content below to the editor. This policy allows download, upload and list objects of S3 Bucket in all S3 Buckets within AWS. Then, click **Create endpoint** button.
    
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

4. Verify result
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/005-s3gw.png?width=90pc)

5. Copy and save the VPC endpoint ID to create S3 Bucket Policy in the later steps.
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/006-s3gw.png?width=90pc)

6. Notice that AWS will automatically create a route to the VPC Gateway endpoint in all attached route tables above
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/007-s3gw.png?width=90pc)

#### Create Bucket Policy to S3 Bucket
1. Click to the **my-online-shop-bucket** in the S3 Management Console
    ![S3](/images/2-preparation/2.2-storage/2.2.1-s3/008-s3.png?width=90pc)

2. Choose the **Permissions** section and navigate to the Bucket policy section. Then, click to **Edit**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/008-s3gw.png?width=90pc)

3. Copy and paste the content below to the box and click **Save changes**. This policy only allows connections from VPC Endpoint that we've created above.
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
In the later steps, we will use AWS CLI to copy files from S3 Bucket which requires AWS Account authentication and authorization. The easiest way is to create ACCESS_KEY and SECRET_ACCESS_KEY and store them in the settings of AWS CLI using **aws configure** command. However, this approach poses a security risk since the keys can be revealed to other people. Alternatively, we will create an IAM Role and attach to EC2 Instance so that we can use AWS CLI to interact with S3 Bucket without using access keys.
{{% /notice %}}

1. In the [IAM Management Console](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home)
    + Choose **Roles**
    + Click to **Create role** button
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/010-s3gw.png?width=90pc)

2. Select **EC2 Instance** as Trusted Entity so that EC2 Instance can assume the role and permissions within this role
    + Trusted entity type: **AWS service**
    + Use case: **EC2**
    + Click to **Next**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/011-s3gw.png?width=90pc)

3. Browse for ```AmazonS3ReadOnlyAccess``` policy and tick to it. Then, click **Next**
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/012-s3gw.png?width=90pc)

4. Name, review and create IAM Role
    + Role name: ```AllowEC2ReadS3```
    + Description: ```Allows EC2 instances to read S3 Bucket.```
    + Other options keep default
    + Click to **Create role** button
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/013-s3gw.png?width=90pc)
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/014-s3gw.png?width=90pc)

5. Verify that the IAM Role is succefully created
    ![S3](/images/2-preparation/2.2-storage/2.2.2-s3gwe/015-s3gw.png?width=90pc)