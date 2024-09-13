---
title : "Connect to Development EC2"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

- **Create stack and load into .yaml file to get preconfigured resources**
    
    ![image.png](PhamHuuTuan_Workshop/images/setup_environment/setup_ec2/image.png)
    
- **Retrieve Credentials for your AWS provided development EC2 instance**
    - Once you have accessed the AWS console, enter Secrets in the search box at the top of the page. Under Services, an entry for Secrets Manager will be visible. Click it to open the AWS Secrets Manager service page.
    
    ![image.png](/images/setup_environment/setup_ec2/image_1.png)
    
    - We will see **EC2InstanceSecret**, it appears because we loaded it from .yaml file, now we click on it.
    
    ![image.png](/images/setup_environment/setup_ec2/image_2.png)
    
    - After clicking, scroll down to the Secret value section and then click on **Retreive secret value**.
    
    ![image.png](/images/setup_environment/setup_ec2/image_3.png)
    
    - Then we can see the secret values ​​like User name and Password, copy it to prepare for the next step.
    
    ![image.png](/images/setup_environment/setup_ec2/image_4.png)
    
- **Set up Inbound rule for EC2 Security group for RDP from our device.**
    - Open **AWS console**, search for **EC2** in the search bar and select it.
    
    ![image.png](/images/setup_environment/setup_ec2/image_5.png)
    
    - In **EC2 Dashboard**, select **Instances** **(Running).**
    
    ![image.png](/images/setup_environment/setup_ec2/image_6.png)
    
    - Click on the ID of this running instance.
    
    ![image.png](/images/setup_environment/setup_ec2/image_7.png)
    
    - Scroll down, and select the **Security** tab, then Click on the link of **Security group**
        
        ![image.png](/images/setup_environment/setup_ec2/image_8.png)
        
    - After entering **Security group**, scroll down and select **Inbound rules**, then select **Edit inbound rules**.
        
        ![image.png](/images/setup_environment/setup_ec2/image_9.png)
        
    - We will **Add rule** for **Inbound rule**, specifically we will add 1 rule for protocol **RDP** and the source will come from the IP address of our device, then click **Save rules**.
        
        ![image.png](/images/setup_environment/setup_ec2/image_10.png)
        
- **Log in to our EC2 instance.**
    - Go back to our EC2 instance, tick it and click **Connect**
    
    ![image.png](/images/setup_environment/setup_ec2/image_11.png)
    
    - Select the **RDP client** tab, and then click D**ownload remote desktop file.**
    
    ![image.png](/images/setup_environment/setup_ec2/image_12.png)
    
    - Go to where you just downloaded the file, select it, and proceed to **Connect** with the **user name** and **password** you just copied.
    
    ![image.png](/images/setup_environment/setup_ec2/image_13.png)
    
    ![image.png](/images/setup_environment/setup_ec2/image_14.png)
    
    - We may see a warning like the one below, if so, click Yes.
    
    ![image.png](/images/setup_environment/setup_ec2/image_15.png)
    
    - After successfully logging in, we will use this computer to continue, this is the practice interface of the machine after we successfully log in.
    
    ![image.png](/images/setup_environment/setup_ec2/image_16.png)