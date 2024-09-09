---
title : "Cấu hình MySQL Workbench để kết nối tới RDS"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 1.4 </b> "
---

- Chúng ta cần phải lấy được chi tiết thông tin của **RDS instace** để có thể thiết lập với **MySQL Workbench.**
- **Truy xuất thông tin của RDS MySQL instance được cấp**
    - Trong AWS console, vào thanh tìm kiếm có **Secrets**, sau đó chúng ta bấm chọn **Secrets Manager.**
    
    ![image.png](/images/setup_environment/config_mysql/image.png)
    
    - Sau đó chúng ta bấm chọn vào Secret có tên là **RDSSecretForApp.**
    
    ![image.png](/images/setup_environment/config_mysql/image_1.png)
    
    - Lướt xuống phần **Secret value**, sau đó bấm nút **Retrieve secret value**.
    
    ![image.png](/images/setup_environment/config_mysql/image_2.png)
    
    - Chúng ta có chi tiết về RDS ở đây, copy chúng ra để thực hiện cho lần sau
    
    ![image.png](/images/setup_environment/config_mysql/image_3.png)
    
- **Thiết lập kết nối MySQL Workbench**
    - Tìm MySQL trong cái máy chúng ta kết nối tới instance.
    
    ![image.png](/images/setup_environment/config_mysql/image_4.png)
    
    - Sau khi vào, chọn vào **Database**, sau đó chọn **Manage Connections**.
    
    ![image.png](/images/setup_environment/config_mysql/image_5.png)
    
    - Sau đó bấm vào nút New ở dưới cùng, sau đó nhập tên cho Connection, và sau đó lấy các thông tin từ RDS instance để viết vào, sau đó bấm vào Store in Vault…
    
    ![image.png](/images/setup_environment/config_mysql/image_6.png)
    
    - Sau khi bấm, chúng ta lấy mật khẩu từ chi tiết của RDS để tiến hành nhập vào, sau đó bấm **OK**
    
    ![image.png](/images/setup_environment/config_mysql/image_7.png)
    
    - Sau khi xong, chúng ta bấm **Test Connection** để kiểm tra việc kết nối có được hay không. Nếu nó hiện ra bảng như thế này thì chúng ta đã thiết lập thành công.
    
    ![image.png](/images/setup_environment/config_mysql/image_8.png)