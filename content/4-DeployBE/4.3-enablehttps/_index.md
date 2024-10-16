---
title : "Enable SSL/TLS termination for ALB of Backend"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---
{{% notice note %}}
We just need to enable the SSL/TLS termination at the ALB level which means the ALB will handle the encryption and decryption of HTTPS traffic between ALB and clients. Since the Backend application is deployed in a private subnet of VPC which poses less security risks, we're fine with unencrypted traffic between ALB and backend targets. As a result, we don't need to enable HTTPS for Backend Target Group unless we're required to have **end-to-end encryption** based on the company regulations.
{{% /notice %}}

#### Add HTTPS Inbound rule for Load Balancer security group
1. In EC2 Management dashboard
    + Choose the **Security Group** section in the left sidebar
    + Choose the **Backend-LB-SG** security group
    + Choose **Edit inbound rules** option
    ![BE](/images/4-backend/4.3-https/001-https-be.png?width=90pc)

2. Add Inbound rule
    + Type: **HTTPS**
    + Source: **Anywhere-IPv4**
    + Click **Save rules**
    ![BE](/images/4-backend/4.3-https/002-https-be.png?width=90pc)

#### Request a SSL/TLS certificate for Backend Load Balancer domain
1. In the [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1)
    + Type ```ACM``` in the search bar
    + Choose AWS ACM service
    ![BE](/images/4-backend/4.3-https/003-https-be.png?width=90pc)

2. In the AWS Certificate Manager dashboard, click to **Request a certificate** button
{{% notice note%}}
Note that there is no charge for public SSL/TLS certificate registration with AWS Certificate Manager.\
Reference: [AWS Certificate Manager Pricing](https://aws.amazon.com/certificate-manager/pricing/)
{{% /notice %}}
    ![BE](/images/4-backend/4.3-https/004-https-be.png?width=90pc)

3. Click **Next** to create a public SSL/TLS certificate
    ![BE](/images/4-backend/4.3-https/005-https-be.png?width=90pc)

4. Specify information as below to register a public SSL/TLS certificate
    + FQDN: ```api.longtruong-lab.online``` **(Change the FQDN according to your domain)**
    + Validation method: **DNS validation**
    + Key algorithm: **RSA 2048**
    + Click to **Request**
    ![BE](/images/4-backend/4.3-https/006-https-be.png?width=90pc)

5. A certificate with "Pending validation" status is created
{{% notice note %}}
With DNS validation method, AWS will create one or more CNAME Records depending on the FQDN that we provided. As a result, we have to add this record to Route 53 Hosted Zone and we only need to do this once. After that, the status of certificate is "Issued". 
{{% /notice %}}
    + Click to the **Create record in Route 53 button**
    ![BE](/images/4-backend/4.3-https/007-https-be.png?width=90pc)

6. Since Route 53 and AWS Certificate Manager connect closely together, we don't need to specify additional information. Just click to **Create records** button.
    ![BE](/images/4-backend/4.3-https/008-https-be.png?width=90pc)

7. After a few minutes, the status of the certificate will be "Issued"
    ![BE](/images/4-backend/4.3-https/009-https-be.png?width=90pc)

#### Create HTTPS Listener and rules for Backend Load Balancer
1. In the EC2 Management dashboard
    + Choose **Load Balancers** in the left sidebar
    + Choose **Online-Shop-BE-LB** load balancer
    + Switch to Listeners and rules tab below
    + Choose **Add listener**
    ![BE](/images/4-backend/4.3-https/010-https-be.png?width=90pc)

2. In the Lister details, configure the following parameters 
    + Protocol: **HTTPS**
    + Port: **443**
    + Routing actions: Forward to target groups
    + Target group: **Backend-TG**
    ![BE](/images/4-backend/4.3-https/011-https-be.png?width=90pc)

3. In the secure listener settings section
    + Certificate source: **From ACM**
    + Certificate: **api.longtruong-lab.online** (Choose your DNS domain)
    + Click to **Save changes**.
    ![BE](/images/4-backend/4.3-https/012-https-be.png?width=90pc)

4. At the moment, we have a HTTPS listener in our Backend Load Balancer
    ![BE](/images/4-backend/4.3-https/013-https-be.png?width=90pc)

5. Access to the Backend application with HTTPS protocol
    + The URL: https://api.longtruong-lab.online/swagger/index.html
    ![BE](/images/4-backend/4.3-https/014-https-be.png?width=90pc)