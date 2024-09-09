---
title : "Module 1 - API tìm chuyến bay (Find Flights API)"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Sơ lược về API tìm chuyến bay

- Trong module này, bạn sẽ xây dựng API để tìm chuyến bay dựa trên ngày khởi hành, mã sân bay khởi hành và mã sân bay đến. API này trả về danh sách các chuyến bay có thông tin chi tiết về chuyến bay, chỗ ngồi và thông tin về giá. Lệnh gọi API được bảo mật bằng JWT token. Người dùng đăng nhập vào UI người dùng được hosted trên Cognito để nhận JWT token và chuyển token trong Authorization HTTP header. API xác minh JWT token và sau đó cho phép thực hiện hành động được yêu cầu. API tương tác với các bảng chuyến bay (flights)  và sân bay (airport) trong cơ sở dữ liệu RDS MySQL để nhận kết quả. Thông tin đăng nhập RDS được lưu trữ an toàn trong AWS Secret Manager.
- API trả về các trường sau:
    - Flight Id, Departure, Departure, Departure AirportCode, Departure Airport Name, departure Airport City, Departure Airport Locale Arrival Airport Code, Arrival Airport Name, Arrival Airport City, Arrival Airport Locale, Arrival Date, Arrival Time, Ticket Price Ticket Currency, Flight Number, Flight Duration; Seat Available
- Cơ sở dữ liệu cho module này có 2 bảng:
    - Bảng Airport- lưu trữ thông tin chi tiết về Sân bay với Airport Code làm khóa chính và thông tin chi tiết về sân bay liên quan.
    - Bảng Flight - lưu trữ danh sách các chuyến bay cùng với lịch trình, sức chứa và số ghế còn trống.
- Sơ đồ lớp
  ![image](/images/module_1/sodolop.png?width=50pc&classes=shadow)
  

#### Liên kết nhanh đến các phần hướng dẫn chi tiết

1. [Thiết lập bảng cho database của Module 1](1-table-database-module1)
2. [Xây dựng các lớp model](2-model-classes-module1)
3. [Tạo Data Transfer Object (DTO)](3-dto-module1)
4. [Dựng JPA Repository Interface](4-jpa-repository-interface-module1)
5. [Cấu hình Datasource](5-config-datasource-module1)
6. [Xây dựng các lớp Service](6-service-classes-module1)
7. [Unit Testing cho các lớp Reposity](7-unit-test-repository-classes-module1)
8. [Unit Test lớp service](8-unit-test-service-classes-module1)
9. [Lấy Cognito public keys](9-get-pkeys-module1)
10. [Xây dựng Exception Handler để xử lý các Authentication Exceptions](10-exception-handler-module1)
11. [Tạo lớp FlightReservation Controller](11-flight-reservations-controller-classes-module1)
12. [Unit test API tìm chuyến bay](12-unit-test-find-flights-api)
