---
title : "Cấu hình Cognito"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---


- **Tìm hiểu về Amazon Cognito**
    - Một Amazon Cognito user pool cung cấp một thư mục người dùng mà bạn sẽ sử dụng để xác thực và cấp quyền truy cập vào API ứng dụng đặt chỗ chuyến bay. Bạn sẽ sử dụng UI người dùng được host trên Amazon Cognito để đăng ký và tạo người dùng mới. Nó sẽ trả về session token (JW Token), cái này thì bạn sẽ sử dụng trong HTTP request Authorization headers khi bạn thực hiện lệnh gọi API. Sau này trong workshop ở module 1, chúng ta sẽ viết mã để xác minh token được phép trong request API bằng cognito. Thực hiện theo Hướng dẫn để đăng ký và tạo người dùng mới. Hành động đăng ký sẽ tự động đăng nhập cho bạn và cung cấp cho bạn token. Các token này được định cấu hình để có hiệu lực trong 1 giờ. Nếu bạn gặp lỗi hết hạn token trong quá trình kiểm tra API, hãy làm theo hướng dẫn bên dưới để đăng nhập và nhận token mới.
- **Đăng ký tạo người dùng mới**
    - Gõ vào **Cognito** trong ô tìm kiếm và sau đó chọn vào
    
    ![image.png](/images/setup_environment/config_cognito/image.png)
    
    - Trong mục **User pools**, chúng ta sẽ thấy có 1 User pool được thiết lập sẵn, phần đuôi của nó sẽ là **-UserPool**, hãy chọn vào nó.
    
    ![image.png](/images/setup_environment/config_cognito/image_1.png)
    
    - Sau khi bấm vào, chọn vào tab **App integration.**
    
    ![image.png](/images/setup_environment/config_cognito/image_2.png)
    
    - Tiếp theo lăn xuống mục **App clients and analytics** và click vào App client mà bạn đã được thiết lập sẵn.
    
    ![image.png](/images/setup_environment/config_cognito/image_3.png)
    
    - Trong màn hình của **App client,** lăn xuống mục **Hosted UI,** sau đó bấm vào **View Hosted UI.**
    
    ![image.png](/images/setup_environment/config_cognito/image_4.png)
    
    - Sau khi bấm vào, chúng ta sẽ được đưa đến giao diện đăng nhập, bấm vào Sign up để tiến hành đăng ký 1 tài khoản, và chúng ta tiến hành đăng ký.
    
    ![image.png](/images/setup_environment/config_cognito/image_5.png)
    
    - Sau đó chúng ta nhận được mã để xác minh tài khoản trong mail mà chúng ta đăng ký, xác minh xong thì chúng ta sẽ nhận được.
    
    ![image.png](/images/setup_environment/config_cognito/image_6.png)
    
    - Chúng ta không cần quan tâm, thứ chúng ta quan tâm là url ở trên thanh tìm kiếm, lúc này trên thanh của chúng ta sẽ có token, chúng ta copy cái URL ra, tìm thấy giá trị của access token và copy lấy nó.
    - Lưu ý là token này chỉ tồn tại trong vòng 1 giờ, sau 1 giờ thì chúng ta tiếp tục đăng nhập lại và lấy token.
    
    ![image.png](/images/setup_environment/config_cognito/image_7.png)