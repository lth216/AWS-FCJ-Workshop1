---
title : "Test scalability of Frontend"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

#### Simulate user requests with Webserver Stress Tool
1. Download and extract [Webserver Stress Tool 8](https://www.paessler.com/tools/webstress)

2. Test the self-scalability of Online Shop application
    + Test type: CLICKS
      + Run until: **100000** Clicks Per User
    + User simulation
      + Number Of Users: **101**
      + Click Delay: **1**

    ![TEST](/images/7-test/7.3-test/001-testasg.png?width=50pc)

3. Copy the DNS name of **Frontend Load Balancer** and paste to the URL column in the **http://** section. Then, click **Start Test**.

  ![TEST](/images/7-test/7.3-test/002-testasg.png?width=50pc)

4. Let the tool process for a few minutes

  ![TEST](/images/7-test/7.3-test/003-testasg.png?width=50pc)

5. After some time, check the Activity history of the Auto Scaling group of Frontend application. At the moment, the number of EC2 Instance is scaled out to 3

  ![TEST](/images/7-test/7.3-test/004-testasg.png?width=50pc)

6. Check the **Frontend Load Balancer**, we can see that all the instances are **Healthy**

  ![TEST](/images/7-test/7.3-test/005-testasg.png?width=90pc)

7. The access to the website is not interrupted

  ![TEST](/images/7-test/7.3-test/006-testasg.png?width=90pc)

8. View the log file result
  
  **We can see that there was no error response although the amount of request escalate by time.**
  ![TEST](/images/7-test/7.3-test/007-testasg.png?width=90pc)

  ![TEST](/images/7-test/7.3-test/008-testasg.png?width=50pc)