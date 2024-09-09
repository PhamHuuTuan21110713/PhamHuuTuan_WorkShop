---
title : "Setup the database tables for Module 1"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

- We will create 2 tables, **Airport** and **Flight** in **FlightReservationDB Database**. We will also create SQL scripts to load test data into the 2 tables.
- In the **Airline-Booking-PromptProject** project folder, expand the “**src**” directory from the left sidebar, navigate to the **src/main/java/com.airlines.catalog** folder and open **DBSetupModule1.sql**
- Next we need to open **MySQL workbench** and switch to the **database** named **FlightReservationDB** , this database has been created in the workshop.

![image.png](/images/module_1/create_table/image.png)

![image.png](/images/module_1/create_table/image_1.png)

- Next we use the following command in the IDE to create the **airport** table.

![image.png](/images/module_1/create_table/image_2.png)

- Then use this Prompt for Amazon Q chat to create the **flight.** table.

![image.png](/images/module_1/create_table/image_3.png)

- From the above steps, we will have the complete as shown below, put them into **MySQL Workbench** to generate tables **airport** and **flight**

![image.png](/images/module_1/create_table/image_4.png)

- Next we will insert **test data** into the **airport** table, we will write the script in the IDE, then copy it to **workbench** to run it.

![image.png](/images/module_1/create_table/image_5.png)

- Next we will ask **Amazon Q chat** to insert test data into the **flight** table with the following prompt.
  
{{< callout type="info" title="Prompt" >}}
Create 5 records in flight table satisfying the following data conditions,<br> 

2 flights between MIA and LAX on a departure date 2023-08-01,<br>

2 flight between LHR and CDG on a departure date 2023-08-01,<br>

1 flight between LHR and CDG,  departure date 2023-08-02,<br>

1 flight between MIA and LAX with available seats as 0 on a departure date 2023-08-02
{{< /callout >}}





![image.png](/images/module_1/create_table/image_6.png)

- We will copy all the scripts into MySQL workbench and run them to insert the data into the table.

- Complete code to load test data into the tables

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

- Complete SQL code for test data loading

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