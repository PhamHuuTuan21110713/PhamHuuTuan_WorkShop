---
title : "Create Tables and load data"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

- Create the tables required for the reservation API. This API needs two tables:
    - Passenger table to hold passenger's personal information.
    - Reservation table to hold the flight reservation details.
- No test data setup is required for these tables.
- Navigate to src/main/java/com.airlines.catalog folder and open the DBSetupModule2.sql file. Use the below prompt to build the SQL script for passenger table

{{< callout type="info" title="Prompt" >}}

Create passenger table with columns passenger_Id Auto increment, adult, gender, first_Name, last_Name
All columns are not  nullable

{{< /callout >}}

![image.png](/images/module_2/table_data/image.png)

- After having the SQL command, we put it through MySQL workbench to run and create the table.
- Use the below prompt to build the SQL script for reservation table:

{{< callout type="info" title="Prompt" >}}

Create reservation table with columns booking_Reference as BIGINT Auto increment, passenger_Id, flight_Id, reservation_Date, reservation_Time, reservation_Status, travel_Class, ticket_Price as decimal, currency_Code, payment_Status, payment_Mode, contact_Number, contact_Email passenger_Id references passenger table flight_Id references flight.id

{{< /callout >}}

- Now we have the 2 tables needed for module 2.

![image.png](/images/module_2/table_data/image_1.png)

- Complete code for SQL scripts

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