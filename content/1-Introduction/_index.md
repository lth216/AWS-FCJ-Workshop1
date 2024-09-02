---
title : "Introduction"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

#### Introduction
In this workshop, we will explore how to set up a scalable, resilient, and secure web hosting environment on AWS by leveraging a combination of AWS services. Below is a brief introduction to the core AWS services we will use:

**AWS VPC (Virtual Private Cloud)** enables you to create a logically isolated network in the AWS cloud. This service gives you control over your virtual networking environment, including IP address ranges, subnets, route tables, and security settings. We will use VPC to define our network architecture and secure our resources.

**AWS EC2 (Elastic Compute Cloud)** provides scalable virtual servers in the cloud, known as instances. These instances allow you to run applications in a secure and resizable compute environment. In this workshop, we will use EC2 instances to host our web server including frontend and backend, which will serve dynamic content to users.

**AWS ALB (Application Load Balancer)** distributes incoming traffic across multiple targets, such as EC2 instances, in one or more Availability Zones. ALB operates at the application layer (Layer 7) and provides advanced routing capabilities, ensuring high availability and fault tolerance for your web application.

**Amazon Auto Scaling Group** automatically adjust the number of EC2 instances to meet the demand for your application. It helps maintain optimal performance by adding or removing instances based on traffic patterns. This workshop will demonstrate how to set up an ASG to ensure your web application can scale dynamically.

**Amazon RDS (Relational Database Service)** is a managed database service that supports several database engines, including MySQL, Microsoft SQL, PostgreSQL, and Oracle. RDS automates time-consuming administrative tasks like backups, patch management, and scaling. In our setup, RDS will be used to host the database backend for our web application.

**Let's dive right in !**