---
title : "Module 2 - API đặt chỗ chuyến bay (Flight Reservation API)"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

### Sơ lược về API đặt chỗ chuyến bay
#### Tổng quan về API

- Trong Module này, chúng ta sẽ xây dựng API đặt chỗ Chuyến bay cung cấp chức năng đặt chuyến bay. API sẽ lấy thông tin chi tiết về hành khách, thông tin đặt chỗ và thông tin chi tiết về chuyến bay để đặt chỗ làm đầu vào. Id chuyến bay được trả về từ lệnh gọi API **findFlight** sẽ được cung cấp làm đầu vào cho API này. Lệnh gọi API được bảo mật bằng JWT token. Người dùng sẽ đăng nhập vào UI người dùng được host trên Cognito để nhận JWT token và pass token trong Authorization HTTP header. API xác minh token trước khi lưu dữ liệu vào bảng hành khách (**passenger**) và đặt chỗ (**reservation**) trong cơ sở dữ liệu. API sẽ thực hiện các chức năng sau:
    - Xác thực các chi tiết đặt chỗ được cung cấp trong yêu cầu, chẳng hạn như Not Null, xác thực định dạng Email và xác thực định dạng Số điện thoại.
    - Lưu trữ dữ liệu sau khi xác nhận vào bảng hành khách (**passenger**) và đặt chỗ (**reservation**). Gửi thông báo xác nhận đặt chỗ với các chi tiết đặt chỗ và số tham chiếu đặt chỗ đến SNS topic. Toàn bộ bước này cần phải xảy ra trong một atomic transaction.
    - Người dùng nhận được số tham chiếu đặt chỗ qua email và cả trong phản hồi API.

#### Class design cho module 2

![image.png](/images/module_2/class_design.png?classes=shadow)

### Liên kết nhanh đến các phần hướng dẫn chi tiết

1. [Tạo bảng và load dữ liệu](1-table-database-module2)
2. [Xây dựng tầng model](2-model-classes-module2)
3. [Xây dựng JPA Repository](3-jpa-repository-module2)
4. [Unit test cho các lớp Repository ](4-unit-test-repository-classes-module2)
5. [Xử lý các Business Exceptions](5-handle-business-exceptions-module2)
6. [Xây dựng các lớp service](6-service-classes-module2)
7. [Unit Test Lớp service FlightBooking ](7-unit-test-flightbooking-service-classes)
8. [Tạo chi tiết đặt chỗ DTO (DTO Reservation Details)](8-dto-reservation-details)
9. [Tạo Controller API đặt chuyến bay](9-book-flight-api-controller)
10. [Unit test cho  API đặt chỗ chuyến bay](10-unit-test-reserve-flight-api)