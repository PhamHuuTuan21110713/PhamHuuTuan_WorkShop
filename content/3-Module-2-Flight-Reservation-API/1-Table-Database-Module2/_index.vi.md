---
title : "Tạo bảng và load dữ liệu"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

- Tạo các bảng cần thiết cho API. API này cần hai bảng:
    - Bảng khách hàng (**Passenger**) nắm giữ thông tin cá nhân của hành khách.
    - Bảng đặt chỗ (**Reservation**) nắm giữ chi tiết đặt chỗ chuyến bay.
- Chúng ta không cần thiết lập dữ liệu test cho các bảng này.
- Điều hướng đến thư mục **src/main/java/com.airlines.catalog** và mở tệp **DBSetupModule2.sql**. Sử dụng prompt bên dưới để xây dựng tập lệnh SQL cho bảng **passenger.**

{{< callout type="info" title="Prompt" >}}

Create passenger table with columns passenger_Id Auto increment, adult, gender, first_Name, last_Name
All columns are not  nullable

{{< /callout >}}

![image.png](/images/module_2/table_data/image.png)

- Sau khi có lệnh SQL thì chúng ta bỏ nó qua MySQL workbench để tiến hành chạy và tạo bảng
- Tương tự như trên, dùng prompt này để tạo bảng **reservation**

{{< callout type="info" title="Prompt" >}}

Create reservation table with columns booking_Reference as BIGINT Auto increment, passenger_Id, flight_Id, reservation_Date, reservation_Time, reservation_Status, travel_Class, ticket_Price as decimal, currency_Code, payment_Status, payment_Mode, contact_Number, contact_Email passenger_Id references passenger table flight_Id references flight.id

{{< /callout >}}

- Bây giờ chúng ta đã có 2 bảng cần cho module 2.

![image.png](/images/module_2/table_data/image_1.png)

- Code SQL tạo bảng hoàn chỉnh

```sql
// Create passenger table with columns passenger_Id as int Auto increment, adult, gender, first_Name, last_Name
// All columns are not  nullable
CREATE TABLE passenger (
    passenger_id INT NOT NULL AUTO_INCREMENT,
    adult BOOLEAN NOT NULL,
    gender VARCHAR(10) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    PRIMARY KEY (passenger_id)
);

// Create reservation table with columns booking_Reference as BIGINT Auto increment, passenger_Id, flight_Id,
// reservation_Date, reservation_Time, reservation_Status, travel_Class, ticket_Price as decimal,
// currency_Code, payment_Status,
// payment_Mode, contact_Number, contact_Email
// passenger_Id references passenger table
// flight_Id references flight.id

CREATE TABLE reservation (
    booking_reference BIGINT NOT NULL AUTO_INCREMENT,
    passenger_id INT NOT NULL,
    flight_id INT NOT NULL,
    reservation_date DATE NOT NULL,
    reservation_time TIME NOT NULL,
    reservation_status VARCHAR(20) NOT NULL,
    travel_class VARCHAR(20) NOT NULL,
    ticket_price DECIMAL(10,2) NOT NULL,
    currency_code VARCHAR(3) NOT NULL,
    payment_status VARCHAR(20) NOT NULL,
    payment_mode VARCHAR(20) NOT NULL,
    contact_number VARCHAR(20) NOT NULL,
    contact_email VARCHAR(50) NOT NULL,
    PRIMARY KEY (booking_reference),
    FOREIGN KEY (passenger_id) REFERENCES passenger(passenger_id),
    FOREIGN KEY (flight_id) REFERENCES flight(id)
);
```