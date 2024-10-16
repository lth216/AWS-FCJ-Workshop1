---
title : "Deploy AWS CloudFront"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---

#### Create CloudFront distribution
1. In the [AWS Management Dashboard](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Type ```CloudFront``` in the search dashboard
    + Choose CloudFront service
    ![CloudFront](/images/6-connect/6.2-cloudfront/001-cloudfront.png?width=90pc)

2. In the CloudFront dashboard, click to **Create CloudFront Distribution**
    ![CloudFront](/images/6-connect/6.2-cloudfront/002-cloudfront.png?width=90pc)

3. Configure cloufront distribution
    + Origin:
      + Origin domain: **online-shop-fe-lb-681861540.us-east-1.elb.amazonaws.com** (Use domain of Application Load Balancer of Frontend)
      + Protocol: **HTTPS only**
      + Other options keep default
      ![CloudFront](/images/6-connect/6.2-cloudfront/003-cloudfront.png?width=90pc)
    + Default cache behavior
      + Viewer protocol policy: **Redirect HTTP to HTTPS**
      + Allowed HTTP methods: **GET**, **HEAD**
      + Other options keep default
      ![CloudFront](/images/6-connect/6.2-cloudfront/004-cloudfront.png?width=90pc)
    + Web Application Firewall (WAF)
      + Check to **Do not enable security protections**
      ![CloudFront](/images/6-connect/6.2-cloudfront/005-cloudfront.png?width=90pc)
    + Settings
      + Price class: **Use all edge locations (best performance)**
      + Custom SSL certificate: Choose the longtruong-lab.online certificate that we've created before
    + Other options keep default
      ![CloudFront](/images/6-connect/6.2-cloudfront/006-cloudfront.png?width=90pc)
    + Click to **Create distribution** button
      ![CloudFront](/images/6-connect/6.2-cloudfront/007-cloudfront.png?width=90pc)

4. Wait for CloudFront to initialize. It can take time depend on the number of edge locations.
    ![CloudFront](/images/6-connect/6.2-cloudfront/008-cloudfront.png?width=90pc)

5. When CloudFront finishes downloading the content to edge location, it will display a timeline in the **Last modified** section
    ![CloudFront](/images/6-connect/6.2-cloudfront/009-cloudfront.png?width=90pc)

#### Create a DNS Record for CloudFront
1. In the Route 53 Dashboard, choose **Create record** button
    ![CloudFront](/images/6-connect/6.2-cloudfront/010-cloudfront.png?width=90pc)

2. Configure record
    + Record type: **A**
    + Enable Alias
    + Route traffic to: **Alias to CloudFront distribution**
    + Region: **us-east-1** (N.Virginia) 
    + Choose DNS domain of CloudFront distribution
    + Other options keep default
    + Click **Create records**
    ![CloudFront](/images/6-connect/6.2-cloudfront/011-cloudfront.png?width=90pc)

3. Verify the result
    ![CloudFront](/images/6-connect/6.2-cloudfront/012-cloudfront.png?width=90pc)

4. Access the application through DNS domain attached to CloudFront
    ![CloudFront](/images/6-connect/6.2-cloudfront/013-cloudfront.png?width=90pc)