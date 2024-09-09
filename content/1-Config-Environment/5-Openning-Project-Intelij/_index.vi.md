---
title : "Mở Project trong Intelij IDE"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 1.5 </b> "
---

- Bên trong EC2 instance, hãy mở thư mục **C:/Users/Administrator/IdeaProjects/AppCodeArchive** và bạn sẽ có thể thấy một thư mục **Airline-Booking-PromptProject**. Đây là dự án IntelliJ mà bạn sẽ sử dụng để thực hiện workshop.

![image.png](/images/setup_environment/openning_project/image.png)

- Bây giờ mở dự án đó với Intelij IDE

![image.png](/images/setup_environment/openning_project/image_1.png)

- Từ menu bên trái của Dự án IntelliJ, hãy mở rộng thư mục **src** và đi tới **src/main/resources**. Mở tệp "**application.properties**". Cập nhật **aws.region** với **region** bạn đang chạy workshop và **secretmanager.key** dưới dạng **RDSSecretForApp**

![image.png](/images/setup_environment/openning_project/image_2.png)

- Vào **CloudFormation**, click vào **stack** lúc tạo.

![image.png](/images/setup_environment/openning_project/image_3.png)

- Sau đó chọn qua tab **Outputs**, nhận các giá trị của **cognito.userpool.id** từ trường **CognitoProviderName** (Phần cuối của URL) và **sns.arn** từ trường **SNSTopic**. Cập nhật giá trị cho các khóa này trong tệp **application.properties**.

![image.png](/images/setup_environment/openning_project/image_4.png)

![image.png](/images/setup_environment/openning_project/image_5.png)