---
title : "Unit test cho các lớp Repository "
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---


- Tiếp theo, bạn sẽ xây dựng các unit test cho các lớp Repository - **PassengerRepository** và **DepositRepositoryTest**.
- Mở **PassengerRepositoryTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau

![image.png](/images/module_2/unit_test_repository/image.png)

- Tạo các unit test case cho lớp **PassengerRepository** bằng prompt cho Amazon Q Developer bên dưới.

{{< callout type="info" title="Prompt" >}}

Create Junit test cases for PassengerRepository using
web environment with random port.
Create test case for save method by creating test data for firstName, lastName, adult and gender.
Assert that the passenger Id is not null

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_1.png)

- Chuột phải vào **PassengerRepositoryTest**, sau đó chọn vào **Run PassengerRepositoryTest**, nó sẽ chạy các test case trong lớp.
- Tôi đã pass được test case, và trong database cũng có dữ liệu test

![image.png](/images/module_2/unit_test_repository/image_2.png)

![image.png](/images/module_2/unit_test_repository/image_3.png)

- Tiếp theo chúng ta sẽ test cho lớp còn lại, mở **ReservationRepositoryTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các câu import sau.

![image.png](/images/module_2/unit_test_repository/image_4.png)

- Tạo lớp **ReservedRepository** bằng prompt với Amazon Q Developer sau đây, chúng ta sẽ xóa mọi code liên quan tới phương thức do Amazon Q tạo.

{{< callout type="info" title="Prompt" >}}

Create ReservationRepositoryTest class to test the reservationRepository class using
SpringBootTest with web environment and random port

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_5.png)

- Sử dụng prompt Amazon Q Developer bên dưới để tạo phương thức cho test case đầu tiên

{{< callout type="info" title="Prompt" >}}

Create a test method for successful save with valid data
Attributes; flightId,travelClass, ticketPrice, currencyCode, paymentMode,contactNumber,
contactEmail, reservationStatus, reservationDate, reservationTime, paymentStatus and passengerId.
Assert BookingReference is not null

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_6.png)

- Chuột phải vào **ReservationRepositoryTest**, sau đó chọn vào **Run ReservationRepositoryTest** để nó có thể chạy được toàn bộ test case trong lớp này.
- Tôi đã pass được test case, và dữ liệu test cũng đã xuất hiện trong database

![image.png](/images/module_2/unit_test_repository/image_7.png)

![image.png](/images/module_2/unit_test_repository/image_8.png)

- Code hoàn chỉnh cho lớp **PassengerRepositoryTest**

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;
import org.springframework.test.context.junit4.SpringRunner;
import com.airlines.catalog.model.Passenger;
import com.airlines.catalog.repository.PassengerRepository;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;

/* Create Junit test cases for PassengerRepository using
web environment with random port.
Create test case for save method by creating test data for firstName, lastName, adult and gender.
Assert that the passenger Id is not null
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PassengerRepositoryTest {

    @Autowired
    PassengerRepository passengerRepository;

    @Test
    public void savePassengerTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("Test");
        passenger.setLastName("Test");
        passenger.setAdult(true);
        passenger.setGender("M");
        passengerRepository.save(passenger);
        Assert.assertNotNull(passenger.getPassengerId());
    }
}
```

- Code hoàn chỉnh cho lớp **ReservationRepositoryTest**

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;

import com.airlines.catalog.model.Reservation;
import com.airlines.catalog.repository.ReservationRepository;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;

/*
Create ReservationRepositoryTest class to test the reservationRepository class using
SpringBootTest with web environment and random port
*/

@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ReservationRepositoryTest {

    @Value("${local.server.port}")
    private int port;

    @Autowired
    ReservationRepository reservationRepository;

    /* Create a test method for successful save with valid data
   Attributes; flightId,travelClass, ticketPrice, currencyCode, paymentMode,contactNumber,
   contactEmail, reservationStatus, reservationDate, reservationTime, paymentStatus and passengerId.
   Assert BookingReference is not null
   */
    @Test
    public void testSaveReservation() {
        Reservation reservation = new Reservation();
        reservation.setFlightId(1);
        reservation.setTravelClass("Economy");
        reservation.setTicketPrice(1000);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("Credit Card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("XXXXXXXXXXXXX");
        reservation.setReservationStatus("Confirmed");
        reservation.setReservationDate("2022-01-01");
        reservation.setReservationTime("12:00:00");
        reservation.setPaymentStatus("Paid");
        reservation.setPassengerId(1);
        Reservation savedReservation = reservationRepository.save(reservation);
        Assert.assertNotNull(savedReservation.getBookingReference());
    }
}
```