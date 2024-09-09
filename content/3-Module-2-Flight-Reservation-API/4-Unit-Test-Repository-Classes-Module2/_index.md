---
title : "Unit test the Repository classes"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---


- Next you will build Junit unit tests for the repository classes - PassengerRepository and ReservationRepositoryTest.
- Open PassengerRepositoryTest.java in the folder test/java/com.airlines.catalog.test/ and add the following import statements.

![image.png](/images/module_2/unit_test_repository/image.png)

- Create Unit test cases for PassengerRepository class using the Amazon Q Developer prompt below.
{{< callout type="info" title="Prompt" >}}

Create Junit test cases for PassengerRepository using
web environment with random port.
Create test case for save method by creating test data for firstName, lastName, adult and gender.
Assert that the passenger Id is not null

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_1.png)

- Click on the arrow to the left of PassengerRepositoryTest class declaration and choose Run PassengerRepositoryTest. It will run all the test cases in the class. If any test fails, check the spring boot errors in the IDE. If the exception is because of HibernateJpaConfiguration configuration errors check the application.properties file to ensure right values are populated for aws.region and secretmanager.key. Also ensure the test data satisfies the database constraints in the passenger table.
- I passed the test case, and there is test data in the database.

![image.png](/images/module_2/unit_test_repository/image_2.png)

![image.png](/images/module_2/unit_test_repository/image_3.png)

- Next we will test the remaining class. Open ReservationRepositoryTest.java in the folder test/java/com.airlines.catalog.test/ and add the following import statements.

![image.png](/images/module_2/unit_test_repository/image_4.png)

- Create a **ReservedRepository** class using the following Amazon Q Developer prompt, we will remove all code related to the Amazon Q generated method.

{{< callout type="info" title="Prompt" >}}

Create ReservationRepositoryTest class to test the reservationRepository class using
SpringBootTest with web environment and random port

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_5.png)

- Use the Amazon Q Developer prompt below to create a method for your first test case.

{{< callout type="info" title="Prompt" >}}

Create a test method for successful save with valid data
Attributes; flightId,travelClass, ticketPrice, currencyCode, paymentMode,contactNumber,
contactEmail, reservationStatus, reservationDate, reservationTime, paymentStatus and passengerId.
Assert BookingReference is not null

{{< /callout >}}

![image.png](/images/module_2/unit_test_repository/image_6.png)

- Right click on **ReservationRepositoryTest**, then select **Run ReservationRepositoryTest** so that it can run all test cases in this class.
- I passed the test case, and the test data appeared in the database.

![image.png](/images/module_2/unit_test_repository/image_7.png)

![image.png](/images/module_2/unit_test_repository/image_8.png)

- Complete code for PassengerRepositoryTest class

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

- Complete code for ReservationRepositoryTest class

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