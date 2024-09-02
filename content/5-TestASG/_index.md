---
title : "Test the scalability of Online Shop application"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

#### Test the scalability of Online Shop application
In this section, we will test the scalability of the Online Shop application when there is high number of access requests to the application. In this case, we use a tool called Webserver Stress Tool 8 to simulate the user requests.

1. Download and extract [Webserver Stress Tool 8](https://www.paessler.com/tools/webstress)

2. Test the self-scalability of Online Shop application
    + Test type: CLICKS
      + Run until: **100000** Clicks Per User
    + User simulation
      + Number Of Users: **101**
      + Click Delay: **1**

![TEST](/images/5-testasg/001-testasg.png?width=50pc)

3. Copy the DNS name of **Frontend Load Balancer** and paste to the URL column in the **http://** section. Then, click **Start Test**.

![TEST](/images/5-testasg/002-testasg.png?width=50pc)

4. Let the tool process for a few minutes

![TEST](/images/5-testasg/003-testasg.png?width=50pc)

5. After some time, check the Activity history of the Auto Scaling group of Frontend application. At the moment, the number of EC2 Instance is scaled out to 3

![TEST](/images/5-testasg/004-testasg.png?width=50pc)

6. Check the **Frontend Load Balancer**, we can see that all the instances are **Healthy**

![TEST](/images/5-testasg/005-testasg.png?width=90pc)

7. The access to the website is not interrupted

![TEST](/images/5-testasg/006-testasg.png?width=90pc)

8. View the log file result

![TEST](/images/5-testasg/007-testasg.png?width=90pc)

![TEST](/images/5-testasg/008-testasg.png?width=50pc)