---
title : "Prepare DNS hostname with AWS Route 53"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---
In this workshop, we will use a friendly and memorable host name to access our application instead of using CloudFront or Application Load Balancer domain. When it comes to using DNS domain with AWS Route 53, there are 2 strategies:
- Use Route 53 as the Domain Registrar and Domain Hosting.
- Register a DNS domain with 3rd-party Domain Registrar and use Route 53 as Domain Hosting.

Since I have already had a DNS domain registered with Hostinger before, I will use Route 53 as a Domain Hosting service.

To register a DNS domain with Route 53, please refer here: [Register domain with Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html)

#### Create Hosted Zone in AWS Route 53

{{% notice note %}}
Route 53 Hosted Zone (DNS Name Server) is a container for DNS Records on AWS which define how you want to route traffic to a domain or subdomain.\
For each 25 first hosted zones, we will be charge $0.5 per zone per month.\
Reference: [AWS Route 53 Pricing](https://aws.amazon.com/route53/pricing/)
{{% /notice %}}
1. Create a Public Hosted Zone in Route 53
    + In the [AWS Management Console](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#)
      + Type ```Route 53```to the search bar
      + Choose AWS Route 53 service
      ![Route53](/images/2-preparation/2.4-domain/001-route53.png?width=90pc)

2. Choose **Hosted zones** in the left sidebar and click **Create hosted zone** button
    ![Route53](/images/2-preparation/2.4-domain/002-route53.png?width=90pc)

3. Configure information for the Hosted Zone
    + Domain name: ```longtruong-lab.online```
    + Description (optional): ```Hosted zone used for longtruong-lab.online DNS domain```
    + Type: Public hosted zone
    + Click Create hosted zone button
    ![Route53](/images/2-preparation/2.4-domain/003-route53.png?width=90pc)

4. Now, we have a Hosted Zone for **longtruong-lab.online** domain. AWS automatically create a NS Record which is used to route traffic to this Hosted Zone on AWS. Consequently, we will replace these nameservers with the one of the 3rd-party Domain Registrar to have it routed the traffic to AWS.
    ![Route53](/images/2-preparation/2.4-domain/004-route53.png?width=90pc)

#### Configure Route 53 as Domain Hosting for 3rd party Domain Registrar
{{% notice note %}}
When update the Route 53 Name Servers for the DNS domain at Hostinger, Hostinger will communicate this change to the Top Level Domain (TLD) registry. Consequently, when Local DNS Resolver sends DNS queries to TLD registry, it will check its database and respond with Route 53 nameservers, then the Local DNS Resolver sends queries to Route 53 and get appropriate DNS record, allowing the resolver to send the correct IP address back to the browser.  
{{% /notice %}}

1. Navigate to [Hostinger domains dashboard](https://hpanel.hostinger.com/domains) and click to the target domain 
    ![Route53](/images/2-preparation/2.4-domain/005-route53.png?width=90pc)

2. In the **DNS/Nameservers** section, click to **Edit**
    ![Route53](/images/2-preparation/2.4-domain/006-route53.png?width=90pc)

3. In the **DNS records** section
    + Click to **Change the nameservers** button
    + Choose **Change the nameservers** option
    + Copy Name Servers of Route 53 Public Hosted Zone and paste to this section
    + Click **Rescue** button
    ![Route53](/images/2-preparation/2.4-domain/007-route53.png?width=90pc)

4. It can take upto 24 hours for the domain name to be transmitted to the new nameservers
    ![Route53](/images/2-preparation/2.4-domain/008-route53.png?width=90pc)

5. To check whether domain is updated with new Route 53 nameservers, we can use **dig** command
    ```bash
    dig longtruong-lab.online NS
    ```
    ![Route53](/images/2-preparation/2.4-domain/009-route53.png?width=40pc)