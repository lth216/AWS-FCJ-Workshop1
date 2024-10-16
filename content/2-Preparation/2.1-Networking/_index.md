---
title : "Networking"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

Regarding to Networking infrastructure of this workshop, we will deploy a Virtual Private Cloud in N.Virginia region (us-east-1) to host our application. We'll use 2 Availability Zones with 6 subnets for Frontend, Backend and Database server. Beside, we're gonna configure security groups for our services to meet the expectation.

The architecture of VPC will look like this:
![Networking](/images/additional/networking.png)

#### Content
- [Create VPC](2.1.1-CreateVPC/)
- [Create subnets for Database server](2.1.2-Createsubnet/)
- [Create Security groups](2.1.3-Configuresg/)