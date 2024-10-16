---
title : "Introduction"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

#### Introduction
In this workshop, we will explore how to set up a scalable, resilient, and secure web hosting environment on AWS by leveraging a combination of AWS services. Below is a brief introduction to the core AWS services we will use:

1. Networking

    **AWS VPC (Virtual Private Cloud)** enables you to create a logically isolated network in the AWS cloud. This service gives you control over your virtual networking environment, including IP address ranges, subnets, route tables, and security settings. We will use VPC to define our network architecture and secure our resources.

    **Amazon Route 53** is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination: domain registration, DNS routing, and health checking.

    **AWS Certificate Manager** handles the complexity of creating, storing, and renewing public and private SSL/TLS X.509 certificates and keys that protect your AWS websites and applications. You can provide certificates for your integrated AWS services either by issuing them directly with ACM or by importing third-party certificates into the ACM management system. ACM certificates can secure singular domain names, multiple specific domain names, wildcard domains, or combinations of these. ACM wildcard certificates can protect an unlimited number of subdomains. You can also export ACM certificates signed by AWS Private CA for use anywhere in your internal PKI.

2. Compute

    **AWS EC2 (Elastic Compute Cloud)** provides scalable virtual servers in the cloud, known as instances. These instances allow you to run applications in a secure and resizable compute environment. In this workshop, we will use EC2 instances to host our web server including frontend and backend, which will serve dynamic content to users.

3. Database

    **Amazon RDS (Relational Database Service)** is a managed database service that supports several database engines, including MySQL, Microsoft SQL, PostgreSQL, and Oracle. RDS automates time-consuming administrative tasks like backups, patch management, and scaling. In our setup, RDS will be used to host the database backend for our web application.

4. Scalability

    **AWS ALB (Application Load Balancer)** distributes incoming traffic across multiple targets, such as EC2 instances, in one or more Availability Zones. ALB operates at the application layer (Layer 7) and provides advanced routing capabilities, ensuring high availability and fault tolerance for your web application.

    **Amazon Auto Scaling Group** automatically adjust the number of EC2 instances to meet the demand for your application. It helps maintain optimal performance by adding or removing instances based on traffic patterns. This workshop will demonstrate how to set up an ASG to ensure your web application can scale dynamically.

5. Latency

    **Amazon CloudFront** is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.

    - If the content is already in the edge location with the lowest latency, CloudFront delivers it immediately.

    - If the content is not in that edge location, CloudFront retrieves it from an origin that you've defined—such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server (for example, a web server) that you have identified as the source for the definitive version of your content.

**Let's dive right in !**