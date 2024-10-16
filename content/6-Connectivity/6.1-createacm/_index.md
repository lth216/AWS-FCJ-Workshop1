---
title : "Create SSL/TLS certificate with AWS ACM"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

#### Create SSL/TLS certificate with AWS Certificate Manager
1. In the [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Type ```ACM``` in the search bar
    + Choose AWS ACM service
    ![CloudFront](/images/6-connect/6.1-acm/001-acm.png?width=90pc)

2. In the AWS Certificate Manager dashboard, click to **Request a certificate** button
    ![CloudFront](/images/6-connect/6.1-acm/002-acm.png?width=90pc)

3. Click **Next** to create a public SSL/TLS certificate
    ![CloudFront](/images/6-connect/6.1-acm/003-acm.png?width=90pc)

4. Add information to request a public certifcate for **longtruong-lab.online** domain
    + Fully Qualified domain name: ```longtruong-lab.online```
    + Validation method: **DNS validation**
    + Key algorithm: **RSA 2048**
    + Click **Request**
    ![CloudFront](/images/6-connect/6.1-acm/004-acm.png?width=90pc)

5. A certificate is created with **Pending validation** status
   ![CloudFront](/images/6-connect/6.1-acm/005-acm.png?width=90pc)

6. In the **Domains** section of certificate, choose **Create records in Route 53**
    ![CloudFront](/images/6-connect/6.1-acm/006-acm.png?width=90pc)

7. Choose **Create record**
    ![CloudFront](/images/6-connect/6.1-acm/007-acm.png?width=90pc)

8. It may take 30 minutes or more to validate the domain name. After that, the status of certificate is <span style="color:green">Issued</span>
    ![CloudFront](/images/6-connect/6.1-acm/008-acm.png?width=90pc)

9. In the Public Hosted Zone of AWS Route 53, a new CNAME Record of certificate is added.
    ![CloudFront](/images/6-connect/6.1-acm/009-acm.png?width=90pc)