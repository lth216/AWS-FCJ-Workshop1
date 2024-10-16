---
title : "Enable SSL/TLS termination for ALB of Frontend"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---
{{% notice note %}}
We just need to enable the SSL/TLS termination at the ALB level which means the ALB will handle the encryption and decryption of HTTPS traffic between ALB and clients. Since the Frontend application is deployed in a private subnet of VPC which poses less security risks, we're fine with unencrypted traffic between ALB and frontend targets. As a result, we don't need to enable HTTPS for Frontend Target Group unless we're required to have **end-to-end encryption** based on the company regulations.
{{% /notice %}}

#### Add HTTPS Inbound rule for Load Balancer security group
1. In EC2 Management dashboard
    + Choose the **Security Group** section in the left sidebar
    + Choose the **Frontend-LB-SG** security group
    + Choose **Edit inbound rules** option
    ![FE](/images/5-frontend/5.3-https/001-https-fe.png?width=90pc)

2. Add Inbound rule
    + Type: **HTTPS**
    + Source: **Anywhere-IPv4**
    + Click **Save rules**
    ![FE](/images/5-frontend/5.3-https/002-https-fe.png?width=90pc)

#### Request a SSL/TLS certificate for Frontend Load Balancer domain
1. In the [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Type ```ACM``` in the search bar
    + Choose AWS ACM service
    ![FE](/images/5-frontend/5.3-https/003-https-fe.png?width=90pc)

2. In the AWS Certificate Manager dashboard, click to **Request a certificate** button
{{% notice note%}}
Note that there is no charge for public SSL/TLS certificate registration with AWS Certificate Manager.\
Reference: [AWS Certificate Manager Pricing](https://aws.amazon.com/certificate-manager/pricing/)
{{% /notice %}}
    ![FE](/images/5-frontend/5.3-https/004-https-fe.png?width=90pc)

3. Click **Next** to create a public SSL/TLS certificate
    ![FE](/images/5-frontend/5.3-https/005-https-fe.png?width=90pc)

4. Specify information as below to register a public SSL/TLS certificate
    + FQDN: ```ui.longtruong-lab.online``` **(Change the FQDN according to your use case)**
    + Validation method: **DNS validation**
    + Key algorithm: **RSA 2048**
    + Click to **Request**
    ![FE](/images/5-frontend/5.3-https/006-https-fe.png?width=90pc)

5. A certificate with "Pending validation" status is created
{{% notice note %}}
With DNS validation method, AWS will create one or more CNAME Records depending on the FQDN that we provided. As a result, we have to add this record to Route 53 Hosted Zone and we only need to do this once. After that, the status of certificate is "Issued". 
{{% /notice %}}
    + Click to the **Create record in Route 53 button**
    ![FE](/images/5-frontend/5.3-https/007-https-fe.png?width=90pc)

6. Since Route 53 and AWS Certificate Manager connect closely together, we don't need to specify additional information. Just click to **Create records** button.
    ![FE](/images/5-frontend/5.3-https/008-https-fe.png?width=90pc)

7. After a few minutes, the status of the certificate will be "Issued"
    ![FE](/images/5-frontend/5.3-https/009-https-fe.png?width=90pc)

#### Create HTTPS Listener and rules for Frontend Load Balancer
1. In the EC2 Management dashboard
    + Choose **Load Balancers** in the left sidebar
    + Choose **Online-Shop-FE-LB** load balancer
    + Switch to Listeners and rules tab below
    + Choose **Add listener**
    ![FE](/images/5-frontend/5.3-https/010-https-fe.png?width=90pc)

2. In the Lister details, configure the following parameters 
    + Protocol: **HTTPS**
    + Port: **443**
    + Routing actions: Forward to target groups
    + Target group: **Frontend-TG**
    ![FE](/images/5-frontend/5.3-https/011-https-fe.png?width=90pc)

3. In the secure listener settings section
    + Certificate source: **From ACM**
    + Certificate: **ui.longtruong-lab.online** (Use your own DNS domain)
    + Click to **Save changes**.
    ![FE](/images/5-frontend/5.3-https/012-https-fe.png?width=90pc)

4. At the moment, we have a HTTPS listener in our Frontend Load Balancer
    ![FE](/images/5-frontend/5.3-https/013-https-fe.png?width=90pc)

5. Access to the Frontend application with HTTPS protocol
    + The URL: https://ui.longtruong-lab.online
    ![FE](/images/5-frontend/5.3-https/014-https-fe.png?width=90pc)