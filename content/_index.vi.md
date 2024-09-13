---
title : "Home"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Amazon Q Developer Workshop - Build enterprise Java application with Spring Boot

#### Tổng quan
- Workshop này nhằm giúp chúng ta học cách xây dựng một ứng dụng Java doanh nghiệp theo kiến ​​trúc microservices bằng Spring Boot, với sự hỗ trợ từ Amazon Q Developer, một AI coding companion của Amazon. Chúng ta sẽ phát triển một ứng dụng microservices vừa phải để hiểu cách sử dụng Amazon Q Developer trong các dự án thực tế. Chúng ta không chỉ dừng lại ở việc tạo code mà còn tích hợp với nhiều dịch vụ AWS khác nhau và kiểm thử ứng dụng.

#### Công nghệ sử dụng
- Ứng dụng mà chúng ta xây dựng là ứng dụng đặt chỗ chuyến bay, và ứng dụng sẽ sử dụng kiến trúc Microservices, chúng ta sẽ viết code và sử dụng Intelij IDEA IDE. Ngoài ra cũng cần các kiến thức về Java, Spring Boot, SQL và Junit.
- Chúng ta sẽ học cách sử dụng Amazon Q Developer để thực hiện các tác vụ sau:
    - Tạo Lược Đồ Cơ Sở Dữ Liệu (Create Database Schema)
    - Tải Dữ Liệu Vào Các Bảng (Load Data into Tables)
    - Triển Khai Business Logic Cho API (Implement the Business Logic for API)
    - Tích Hợp Các Dịch Vụ AWS (Integrate AWS Services)
    - Tạo Kịch Bản Kiểm Thử Đơn Vị (Create Unit Test Scripts for the Classes)
#### Các module của ứng dụng

- Ứng dụng của chúng ta sẽ bao gồm 2 Modules, bao gồm:
    1. **Tìm chuyến bay (Find flights)**
    - API này sẽ nhận danh sách các chuyến bay dựa trên thông số truy vấn ngày khởi hành, thành phố khởi hành và thành phố đến. Trong module này, chúng ta sẽ sử dụng Amazon Q Developer để triển khai tích hợp với Amazon Cognito, AWS Secrets Manager và Amazon RDS bằng Spring Data JPA.
    2. **Đặt chuyến bay (Book flights)**
    - API này sẽ đặt trước chuyến bay và gửi email xác nhận cho hành khách kèm theo chi tiết đặt chỗ. Trong module này, chúng ta sẽ sử dụng Amazon Q Developer để triển khai xác thực dữ liệu đầu vào, ánh xạ dữ liệu, xử lý ngoại lệ, thông báo bằng Amazon SNS. Module 2 tận dụng một số code được xây dựng trong module 1.
  
#### Kiến trúc
![image.png](/images/Kientruc/image.png)
#### Nội dung chính

1. [Thiết lập môi trường](1-config-environment/)
2. [Module 1 - API tìm chuyến bay (Find Flights API)](2-module-1-find-flights-api/)
3. [Module 2 - API đặt chỗ chuyến bay (Flight Reservation API)](3-module-2-flight-reservation-api/)
4. [Chạy thử API với Postman](4-testing-api-with-postman/)
5. [Clean up](5-clean-up/)