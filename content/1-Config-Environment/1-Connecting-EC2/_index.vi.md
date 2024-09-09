---
title : "Kết nối EC2"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

- **Tạo stack và nạp vào file .yaml để có được các tài nguyên đã cấu hình sẵn**
    
    ![image.png](/images/setup_environment/setup_ec2/image.png)
    
- **Truy xuất thông tin xác thực cho EC2 instance đã được cung cấp**
    - Vào AWS Console, nhập vào ô tìm kiếm Secrets, sau đó chọn vào Secrets Manager
    
    ![image.png](/images/setup_environment/setup_ec2/image_1.png)
    
    - Chúng ta sẽ thấy **EC2InstanceSecret**, nó xuất hiện do chúng ta đã nạp từ file .yaml, giờ chúng ta bấm vào nó.
    
    ![image.png](/images/setup_environment/setup_ec2/image_2.png)
    
    - Sau khi bấm vào, kéo xuống phần Secret value và sau đó bấm vào **Retreive secret value**.
    
    ![image.png](/images/setup_environment/setup_ec2/image_3.png)
    
    - Sau đó chúng ta có thể thấy được các giá trị bí mật như là User name và Password, copy nó để chuẩn bị cho bước tiếp theo.
    
    ![image.png](/images/setup_environment/setup_ec2/image_4.png)
    
- **Thiết lập Inbound rule cho Security group của EC2 cho RDP từ thiết bị của chúng ta.**
    - Mở **AWS console**, tìm kiếm **EC2** trên thanh tìm kiếm và chọn vào
    
    ![image.png](/images/setup_environment/setup_ec2/image_5.png)
    
    - Trong **EC2 Dashboard**, chọn vào **Instances** **(Running).**
    
    ![image.png](/images/setup_environment/setup_ec2/image_6.png)
    
    - Click vào ID của instance đang chạy này.
    
    ![image.png](/images/setup_environment/setup_ec2/image_7.png)
    
    - Lăn xuống, và chọn vào tab **Security**, sau đó Click vào link của **Security group**
        
        ![image.png](/images/setup_environment/setup_ec2/image_8.png)
        
    - Sau khi vào được **Security group**, kéo xuống chọn vào **Inbound rules**, sau đó chọn **Edit inboud rules**.
        
        ![image.png](/images/setup_environment/setup_ec2/image_9.png)
        
    - Chúng ta sẽ **Add rule** cho **Inbound rule**, cụ thể chúng ta sẽ thêm 1 rule cho giao thức **RDP** và nguồn sẽ đến từ địa chỉ IP của thiết bị của chúng ta, sau đó bấm **Save rules**.
        
        ![image.png](/images/setup_environment/setup_ec2/image_10.png)
        
- **Đăng nhập vào EC2 instance của chúng ta.**
    - Vào lại EC2 instance của chúng ta, tick chọn vào nó và bấm **Connect**
    
    ![image.png](/images/setup_environment/setup_ec2/image_11.png)
    
    - Chọn vào tab **RDP client**, và sau đó bấm D**ownload remote desktop file.**
    
    ![image.png](/images/setup_environment/setup_ec2/image_12.png)
    
    - Đến nơi vừa tải file, chọn vào nó, và tiến hành **Connect** bằng **user name** và **password** ban nãy vừa sao chép
    
    ![image.png](/images/setup_environment/setup_ec2/image_13.png)
    
    ![image.png](/images/setup_environment/setup_ec2/image_14.png)
    
    - Có thể chúng ta sẽ xuất hiện cảnh báo như hình dưới đây, nếu vậy thì hãy bấm Yes.
    
    ![image.png](/images/setup_environment/setup_ec2/image_15.png)
    
    - Sau khi đăng nhập thành công, chúng ta sẽ sử dụng máy này để tiếp tục, đây là giao diện thực hành của máy sau khi chúng ta đăng nhập thành công.
    
    ![image.png](/images/setup_environment/setup_ec2/image_16.png)