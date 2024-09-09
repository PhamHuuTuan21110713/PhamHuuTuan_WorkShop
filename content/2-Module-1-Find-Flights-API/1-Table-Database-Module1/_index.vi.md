---
title : "Thiết lập bảng cho database của Module 1"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

- Chúng ta sẽ tiến hành tạo 2 bảng đó là **Airport** và bảng **Flight** trong **FlightReservationDB Database**. Chúng ta cũng sẽ tạo các SQL script để tải các data thử nghiệm lên 2 bảng.
- Trong thư mục dự án **Airline-Booking-PromptProject**, mở rộng directory “**src**” từ thanh bên trái, điều hướng đến folder **src/main/java/com.airlines.catalog** và mở **DBSetupModule1.sql**
- Tiếp đến chúng ta cần mở **MySQL workbench** rồi chuyển qua **database** có tên là **FlightReservationDB** , database này đã được tạo sẵn trong workshop.

![image.png](/images/module_1/create_table/image.png)

![image.png](/images/module_1/create_table/image_1.png)

- Tiếp theo chúng ta dùng câu lệnh sau trong IDE để tạo bảng **airport**.

![image.png](/images/module_1/create_table/image_2.png)

- Sau đó dùng Prompt này cho Amazon Q chat để tạo bảng **flight.**

![image.png](/images/module_1/create_table/image_3.png)

- Từ các bước trên, chúng ta sẽ có hoàn chỉnh như hình dưới đây, bỏ chúng vào **MySQL Workbench** để tiến hành generate tạo bảng **airport** và **flight**

![image.png](/images/module_1/create_table/image_4.png)

- Tiếp theo chúng ta sẽ chèn **test data** vào bảng **airport**, chúng ta sẽ viết script trong IDE, rồi sau đó copy qua **workbench** để tiến hành chạy

![image.png](/images/module_1/create_table/image_5.png)

- Tiếp theo chúng ta sẽ nhờ **Amazon Q chat** để chèn test data vào bảng **flight** với prompt như sau.
  
{{< callout type="info" title="Prompt" >}}
Create 5 records in flight table satisfying the following data conditions,<br> 

2 flights between MIA and LAX on a departure date 2023-08-01,<br>

2 flight between LHR and CDG on a departure date 2023-08-01,<br>

1 flight between LHR and CDG,  departure date 2023-08-02,<br>

1 flight between MIA and LAX with available seats as 0 on a departure date 2023-08-02
{{< /callout >}}





![image.png](/images/module_1/create_table/image_6.png)

- Chúng ta sẽ copy tất cả các script vào MySQL workbench rồi chạy để chèn được các data vào bảng.

- Code SQL hoàn chỉnh cho tạo bảng

```sql

use FlightReservationDB;
CREATE TABLE airport (
    airport_code VARCHAR(10) PRIMARY KEY,
    airport_name VARCHAR(100),
    airport_city VARCHAR(100),
    airport_locale VARCHAR(100)
);

CREATE TABLE flight (
    id INT PRIMARY KEY,
    departure_date DATE,
    departure_time TIME,
    departure_airport_code VARCHAR(10),
    arrival_date DATE,
    arrival_time TIME,
    arrival_airport_code VARCHAR(10),
    flight_number VARCHAR(20),
    flight_duration INT,
    ticket_price DECIMAL(10, 2),
    ticket_currency VARCHAR(3),
    seat_capacity INT,
    seat_available INT,
    FOREIGN KEY (departure_airport_code) REFERENCES airport(airport_code),
    FOREIGN KEY (arrival_airport_code) REFERENCES airport(airport_code)
    );
```

- Code SQL hoàn chỉnh cho tải dữ liệu tạm

```sql
INSERT INTO airport (airport_code, airport_name, airport_city, airport_locale) VALUES
('LHR', 'London Heathrow Airport', 'London', 'United Kingdom'),
('MIA', 'Miami International Airport', 'Miami', 'United States'),
('CDG', 'Charles de Gaulle Airport', 'Paris', 'France'),
('LAX', 'Los Angeles International Airport', 'Los Angeles', 'United States');

INSERT INTO flight (id, departure_date, departure_time, departure_airport_code, arrival_date, arrival_time, arrival_airport_code, flight_number, flight_duration, ticket_price, ticket_currency, seat_capacity, seat_available)
VALUES
(1, '2023-08-01', '09:00:00', 'MIA', '2023-08-01', '11:30:00', 'LAX', 'AA101', 330, 199.99, 'USD', 200, 180),
(2, '2023-08-01', '15:00:00', 'MIA', '2023-08-01', '17:30:00', 'LAX', 'AA102', 330, 249.99, 'USD', 180, 150),
(3, '2023-08-01', '08:00:00', 'LHR', '2023-08-01', '10:30:00', 'CDG', 'BA201', 150, 129.99, 'EUR', 220, 200),
(4, '2023-08-01', '16:00:00', 'LHR', '2023-08-01', '18:30:00', 'CDG', 'BA202', 150, 149.99, 'EUR', 180, 160),
(5, '2023-08-02', '10:00:00', 'LHR', '2023-08-02', '12:30:00', 'CDG', 'BA203', 150, 169.99, 'EUR', 220, 220),
(6, '2023-08-02', '12:00:00', 'MIA', '2023-08-02', '14:30:00', 'LAX', 'AA103', 330, 299.99, 'USD', 180, 0);
```