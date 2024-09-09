---
title : "Configure MySQL Workbench to connect to RDS"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 1.4 </b> "
---

- We need to get the details of the **RDS instance** so we can setup it with **MySQL Workbench.**
- **Retrieve the details for your AWS provided development RDS MySQL instance**
    - In the AWS console, go to the search bar with **Secrets**, then we click on **Secrets Manager.**
    
    ![image.png](/images/setup_environment/config_mysql/image.png)
    
    - Then we click on the Secret named **RDSSecretForApp.**
    
    ![image.png](/images/setup_environment/config_mysql/image_1.png)
    
    - Scroll down to the **Secret value** section, then click the **Retrieve secret value** button.
    
    ![image.png](/images/setup_environment/config_mysql/image_2.png)
    
    - We have the RDS details here, copy them for later use.
    
    ![image.png](/images/setup_environment/config_mysql/image_3.png)
    
- **Setup MySQL Workbench Database Connection**
    - Find MySQL in the PC we connect to the instance.
    
    ![image.png](/images/setup_environment/config_mysql/image_4.png)
    
    - Once in, select **Database**, then select **Manage Connections**.
    
    ![image.png](/images/setup_environment/config_mysql/image_5.png)
    
    - Then click on the New button at the bottom, then enter a name for the Connection, and then get the information from the RDS instance to write to, then click on Store in Vault…
    
    ![image.png](/images/setup_environment/config_mysql/image_6.png)
    
    - After clicking, we get the password from the RDS details to enter, then click **OK**
    
    ![image.png](/images/setup_environment/config_mysql/image_7.png)
    
    - Once done, we click **Test Connection** to check if the connection is successful or not. If it shows a table like this, we have successfully set up.
    
    ![image.png](/images/setup_environment/config_mysql/image_8.png)