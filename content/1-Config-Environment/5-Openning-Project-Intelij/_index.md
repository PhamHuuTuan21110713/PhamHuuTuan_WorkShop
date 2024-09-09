---
title : "Open the prompt project in IntelliJ IDE"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 1.5 </b> "
---

- Inside the EC2 instance, open the folder C:/Users/Administrator/IdeaProjects/AppCodeArchive and you should be able to see one the folder Airline-Booking-PromptProject. This has IntelliJ the project that you will use to develop the workshop.

![image.png](/images/setup_environment/openning_project/image.png)

- Now open that project with Intellij IDEA

![image.png](/images/setup_environment/openning_project/image_1.png)

- From the left menu of your IntelliJ Project, expand the **src** folder and go to **src/main/resources**. Open the "**application.properties**" file. Update **aws.region** with the **region** you are running the workshop in and **secretmanager.key** as **RDSSecretForApp**.

![image.png](/images/setup_environment/openning_project/image_2.png)

- Go to **CloudFormation**, click on the **stack** you created.

![image.png](/images/setup_environment/openning_project/image_3.png)

- Then select via the **Outputs** tab, get the values ​​of **cognito.userpool.id** from the **CognitoProviderName** field (Last part of the URL) and **sns.arn** from the **SNSTopic** field. Update the values ​​for these keys in the **application.properties** file.

![image.png](/images/setup_environment/openning_project/image_4.png)

![image.png](/images/setup_environment/openning_project/image_5.png)