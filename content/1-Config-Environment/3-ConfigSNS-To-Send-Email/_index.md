---
title : "Configure SNS Topic to send Emails"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

- **Introduce**
    - The application will send a message to **SNS Topic** - "**reservation-success**" which means booking is successful, once the flight ticket is booked. Now we will configure the topic to send an email whenever a message is sent to this topic.
- **Configure SNS Topic**
    - In the search box, type **SNS**, then click **Simple Notification Noitices.**
    
    ![image.png](/images/setup_environment/config_sns_topic/image.png)
    
    - Select **Topics** in the left Menu bar, then we will see a topic called **reservation-success**, select it.
    
    ![image.png](/images/setup_environment/config_sns_topic/image_1.png)
    
    - After selecting a topic, scroll down to the **Subcriptions** section, then click Create subscription.
    
    ![image.png](/images/setup_environment/config_sns_topic/image_2.png)
    
    - Then we select **Protocol** as **Email**, then we enter the email we want to receive notifications. Finally click **Create subscription.**
    
    ![image.png](/images/setup_environment/config_sns_topic/image_3.png)
    

![image.png](/images/setup_environment/config_sns_topic/image_4.png)