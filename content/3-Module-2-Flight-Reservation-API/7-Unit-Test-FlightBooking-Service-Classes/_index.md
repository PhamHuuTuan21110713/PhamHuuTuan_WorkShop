---
title : "Unit Test FlightBooking service class"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 3.7 </b> "
---


- Now you will build the unit test scripts for FlightBooking service with Junit. Test scripts will integrate with backend services and AWS services or Java classes will not be mocked.
- Open FlightBookingServiceTest.java in the folder test/java/com.airlines.catalog.test/ and add the following import statements.

![image.png](/images/module_2/unit_test_service_flightbooking/image.png)

- Create the class to test the FlightBooking Service

{{< callout type="info" title="Prompt" >}}

Create FlightBookingServiceTest class to test the flightBooking service using web environment with random port.
Autowire sns.arn, aws.region from properties file

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_1.png)
- First test scenario - reservation success with valid input parameters. Reservation should be successful with data saved in passenger, reservation tables and a notification email received.

{{< callout type="info" title="Prompt" >}}

Create reserve flight success test method.
Create Passenger object by setting attributes firstName, lastName, Adult=true and gender=male
create Reservation object class by setting attributes flightId=1,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus,passengerId, reservationDate,
reservationTime, paymentStatus
Create AWS region object

{{< /callout >}}

- Add this code to call the **reserveFlight** **method**

{{< callout type="info" title="Code" >}}

Assert.assertTrue(flightBooking.reserveFlight(passenger, reservation, passengerRepository, reservationRepository, flightRepository, 1, snsArn, region));

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_2.png)
- Second test scenario - reservation failure scenario with invalid flight id as input

{{< callout type="info" title="Prompt" >}}

Create  reserve flight invalid flightId test method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=10000,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId, reservationDate,
reservationTime, paymentStatus
Create AWS region object
asset if the exception contains 'No flights found"


{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_3.png)
- If try / catch block is not added around the reserveFlight method, please add. Amazon Q Developer will provide recommendations line by line

![image.png](/images/module_2/unit_test_service_flightbooking/image_4.png)
- Update the number of seat_available columnn in flight table to 0 for id=5 by running the SQL statement below in MYSQL Workbench.

{{< callout type="info" title="Code" >}}

 update flight set seat_available =0 where id=5; 

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_5.png)
- Third test scenario - reservation failure scenario with no seats available for the requested flight id.

{{< callout type="info" title="Prompt" >}}


Create  reserve flight insufficient seats method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=5,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId,
reservationDate, reservationTime, paymentStatus
Create AWS region object.
asset if the exception contains 'Seats not available".
 

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_6.png)
- Again, if try / catch block is not added around the reserveFlight method, please add. Amazon Q Developer will provide recommendations line by line

![image.png](/images/module_2/unit_test_service_flightbooking/image_7.png)
- We will run the test cases we just created, and I have passed 3 test cases.

![image.png](/images/module_2/unit_test_service_flightbooking/image_8.png)
- Next, we check the database to see if the successful booking test cases have data added to the database. We will check the data in both the passenger and reservation tables, then we will receive a notification email, if the above conditions are met, I have succeeded.

![image.png](/images/module_2/unit_test_service_flightbooking/image_9.png)
![image.png](/images/module_2/unit_test_service_flightbooking/image_10.png)
![image.png](/images/module_2/unit_test_service_flightbooking/image_11.png)
- Complete code for FlightBookingServiceTest class

```java
package com.airlines.catalog.test;

import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.model.Reservation;
import com.airlines.catalog.model.Passenger;
import com.airlines.catalog.repository.ReservationRepository;
import com.airlines.catalog.repository.PassengerRepository;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.service.FlightBooking;
import com.airlines.catalog.exception.FlightNotFoundException;
import com.airlines.catalog.exception.RequestedSeatsNotAvailable;
import software.amazon.awssdk.regions.Region;

/*
Create FlightBookingTest class to test the FlightBooking service using
web environment with random port.
Autowire sns.arn, aws.region from properties file
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightBookingServiceTest {

    @Autowired
    FlightBooking flightBooking;

    @Autowired
    FlightRepository flightRepository;

    @Autowired
    PassengerRepository passengerRepository;

    @Autowired
    ReservationRepository reservationRepository;

    @Value("${sns.arn}")
    String snsArn;

    @Value("${aws.region}")
    String awsRegion;

    /*
    Create reserve flight success test method.
    Create Passenger object by setting attributes firstName, lastName, Adult=true and gender=male
    create Reservation object class by setting attributes flightId=1,travelClass, ticketPrice,
    currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus,passengerId, reservationDate,
    reservationTime, paymentStatus
    Create AWS region object
    */
    @Test
    public void reserveFlightSuccessTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("XXXX");
        passenger.setLastName("XXX");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(1);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("abc@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);

        Assert.assertTrue(flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                reservationRepository, flightRepository, 1, snsArn, region));

    }

    /*
    Create  reserve flight invalid flightId test method.
    Create passenger object by setting attributes firstName, lastName, Adult=true and gender
    Create reservation object class by setting attributes flightId=10000,travelClass, ticketPrice,
    currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId, reservationDate,
    reservationTime, paymentStatus
    Create AWS region object
    asset if the exception contains 'No flights found"
    */
    @Test
    public void reserveFlightInvalidFlightIdTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("XXXX");
        passenger.setLastName("XXX");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(10000);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("abc@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);
        try {
            Assert.assertThrows(FlightNotFoundException.class, () -> flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                    reservationRepository, flightRepository, 10000, snsArn, region));
        } catch (FlightNotFoundException e) {
            Assert.assertEquals("No flights found", e.getMessage());
        }

    }

    /*
Create  reserve flight insufficient seats method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=5,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId,
reservationDate, reservationTime, paymentStatus
Create AWS region object.
asset if the exception contains 'Seats not available".
*/
    @Test
    public void reserveFlightInsufficientSeatsTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("Test");
        passenger.setLastName("Test");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(5);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("test@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);
        try {
            Assert.assertThrows(RequestedSeatsNotAvailable.class, () -> flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                    reservationRepository, flightRepository, 5, snsArn, region));
        } catch (RequestedSeatsNotAvailable e) {
            Assert.assertEquals("Seats not available", e.getMessage());
        }
    }
}
```