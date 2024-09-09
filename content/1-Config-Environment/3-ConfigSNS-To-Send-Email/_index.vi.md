---
title : "Cấu hình SNS topic để gửi email"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

- **Giới thiệu**
    - Ứng dụng sẽ gửi tin nhắn đến **SNS Topic** - "**reservation-success**" nghĩa là đặt chỗ thành công, sau khi vé máy bay được đặt. Bây giờ chúng ta sẽ cấu hình topic để gửi email bất cứ khi nào có tin nhắn được gửi đến topic này.
- **Cấu hình SNS topic**
    - Vào ô tìm kiếm, nhập vào **SNS**, sau đó bấm chọn **Simple Notification Noitices.**
    
    ![image.png](/images/setup_environment/config_sns_topic/image.png)
    
    - Chọn vào phần **Topics** ở thanh Menu bên trái, sau đó chúng ta sẽ thấy có topic là **reservation-success**, hãy tích chọn vào nó.
    
    ![image.png](/images/setup_environment/config_sns_topic/image_1.png)
    
    - Sau khi chọn vào topic, kéo xuống phần **Subcriptions**, sau đó bấm Create subscription.
    
    ![image.png](/images/setup_environment/config_sns_topic/image_2.png)
    
    - Sau đó chúng ta chọn **Protocol** là **Email**, sau đó chúng ta nhập email mà chúng ta muốn nhận được thông báo. Cuối cùng bấm **Create subscription.**
    
    ![image.png](/images/setup_environment/config_sns_topic/image_3.png)
    

![image.png](/images/setup_environment/config_sns_topic/image_4.png)