---
title : "Scalability with Auto-Scaling Group"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

#### Overview
Auto-Scaling group contains a collection of EC2 Instances that are treated as logical grouping for the purposes of automatic scaling and management. 

It starts by launching enough instances to meet its desired capacity. It maintains these instances by performing periodic health checks on the instances in the target group. The Auto Scaling group continues to maintain a fixed number of instances even if an instance become unhealthy by replacing it with another one.

The number of EC2 Instance scaled in/out depends on the scalling policies that we define for the Auto-Scaling group. 
![ASG](/images/7-test/001-asg.png?width=40pc)

#### Target tracking policy of Auto Scaling group
A target tracking policy automatically scales the capacity of your Auto Scaling group based on the target metric value, providing optimal performance and cost efficiency without manual intervention. 

It is possible to combine multiple target tracking policies with different metrics to optimize the scaling performance. The intention of Auto Scaling group is to always prioritize availability.

There are 2 types of target tracking policy in Auto Scaling group: predefined metric and custom metric:
1.  Predefined metric
    + **CPU Utilization**: Adjust the number of EC2 Instances to maintain the specified average CPU utilization percentage across Auto Scaling group
    + **Request Count per Target**: Track the number of requests being routed to the instance via Application Load Balancer and scale based on that number. This ensure that a single instance can only handle a specific amount of traffic
    + **Average Network In/Out**: Adjust the number of EC2 Instances based on the average amount of network traffic (inbound and outbound) accross all instances. 

2. Custom metric
    + **Cloudwatch metrics**: You can choose other available Cloudwatch metrics or your own metrics in Cloudwatch. You must use AWS CLI or SDK to create your own target tracking policy with a customized metric.

#### Choose suitable target tracking policy for Frontend and Backend
API backend application includes CPU-intensive tasks such as processing requests, database queries, etc, resulting in the escalation of CPU utilization. Consequently, the **CPU Utilization policy is the reasonable choice for backend application**.

Frontend application typically handles user interactions, serves static content, makes call to API application. **Request Count per Target would be particularly effective where the number of HTTP requests can vary based on the user's activities**, ensuring that the HTTP/HTTPS requests between instances are balanced.

#### Content
- [Create Auto Scaling group for Backend](7.1-asgbe/)
- [Create Auto Scaling group for Frontend](7.2-asgfe/)
- [Test scalability of Frontend](7.3-test/)