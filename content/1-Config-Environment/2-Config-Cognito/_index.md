---
title : "Set up Cognito"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---


- **Learn about Amazon Cognito**
    - An Amazon Cognito user pool provides an user directory that you will use to authenticate & authorize the access to Flight reservation application APIs. You will the use the Amazon Cognito hosted UI to sign up and create a new user. It will return a session token (JWT Token) which you will use in your HTTP request Authorization headers when you make a API call. Later on in the workshop in module 1, we will write code to verify the token passed in API request with cognito. Follow the Instructions to sign up and a create a new user. Sign up action automatically signs you in and gives you a token. These tokens are configured to be valid for 1 hour. If you get a token expired error in the API testing, follow instructions below to sign in and get a new token.
- **Sign up to create a new user account**
    - Type **Cognito** in the search box and then select it.
    
    ![image.png](/images/setup_environment/config_cognito/image.png)
    
    - In the **User pools** section, we will see a pre-set User pool, its suffix will be **-UserPool**, select it.
    
    ![image.png](/images/setup_environment/config_cognito/image_1.png)
    
    - After clicking, select the **App integration** tab.
    
    ![image.png](/images/setup_environment/config_cognito/image_2.png)
    
    - Next scroll down to **App clients and analytics** and click on the App client that you have already set up.
    
    ![image.png](/images/setup_environment/config_cognito/image_3.png)
    
    - In the **App client screen,** scroll down to **Hosted UI,** then click **View Hosted UI.**
    
    ![image.png](/images/setup_environment/config_cognito/image_4.png)
    
    - After clicking, we will be taken to the login interface, click Sign up to register an account, and we proceed to register.
    
    ![image.png](/images/setup_environment/config_cognito/image_5.png)
    
    - Then we receive the code to verify the account in the email we registered, after verifying we will receive it.
    
    ![image.png](/images/setup_environment/config_cognito/image_6.png)
    
    - We don't need to care, what we care about is the url on the search bar, now on our bar there will be a token, we copy the URL, find the value of the access token and copy it.
    - Note that this token only exists for 1 hour, after 1 hour we continue to log in again and get the token.
    
    ![image.png](/images/setup_environment/config_cognito/image_7.png)