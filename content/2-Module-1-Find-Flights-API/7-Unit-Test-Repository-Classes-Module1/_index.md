---
title : "Unit Testing of the repository classes"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

- Now we will unit test the repository classes. The testing will performed by direct integration with RDS (No Mocking).
- Open AirportRepositoryTest.java under the test/java/com.airlines.catalog.test/ folder and add the following imports.

![image.png](/images/module_1/unit_test_repository/image.png)

- Use the prompt below to build unit test cases for AirportRepository class

{{< callout type="info" title="Prompt" >}}

*Create AirportRepositoryTest class to test the AirportRepository class using web environment with random port.*<br>

*First negative test case: method with Invalid airport code - "LHG" which should assert a null object*<br>

*Second positive test case: method with valid airport code - "LHR" which should assert a not null object*<br>

{{< /callout >}}

![image.png](/images/module_1/unit_test_repository/image_1.png)

- Click on the arrow to the left of AirportRepositoryTest class declaration and choose Run AirportRepositoryTest. It will run all the test cases in the class. If any test fails, check the spring boot errors in the IDE. If the exception is because of HibernateJpaConfiguration configuration errors check the "application.properties" file to ensure right values are populated for aws.region and secretmanager.key. Also check if the security group for RDS has allowed communication from the IP of the machine where this program is running. If Spring Boot starts successfully then check assertion code and verify that the test data in the airport table is populated according to the test case.
- I ran and passed all the test cases.

![image.png](/images/module_1/unit_test_repository/image_2.png)

- Open FlightRepositoryTest.java under the test/java/com.airlines.catalog.test/ folder and add the following imports

![image.png](/images/module_1/unit_test_repository/image_3.png)

- Build Unit test cases for FlightRepository class using the prompt below. Change the test data values in prompt based on the data you loaded previously into the flight table.

{{< callout type="info" title="Prompt" >}}

*Create FlightRepositoryTest class to test the FlightRepository class using web environment with random port.*<br>

*Test cases for testing findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode method that returns an iterable.*<br>

*First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG" and arrival airport code as "LAX".*<br>

*Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR" and arrival airport code as "CDG" .*<br>

*Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "MIA" and arrival airport code as "LAX".*<br>

{{< /callout >}}

![image.png](/images/module_1/unit_test_repository/image_4.png)

- Click on the arrow to the left of FlightRepositoryTest class declaration and choose Run FlightRepositoryTest. It will run all the test cases in the class. If any test fails, check the spring boot errors in the IDE. If the exception is because of HibernateJpaConfiguration configuration errors check the "application.properties" file to ensure right values are populated for aws.region and secretmanager.key. If Spring Boot starts successfully then check assertion code and verify that the test data in the flight table is populated according to the test case.
- I ran and all 3 test cases passed.

![image.png](/images/module_1/unit_test_repository/image_5.png)

- Complete code for FlightRepositoryTest class

```java
package com.airlines.catalog.test;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.model.Flight;
import com.airlines.catalog.repository.FlightRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

/*
Create FlightRepositoryTest class to test the FlightRepository class using web environment with random port.
Test cases for testing findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode method that returns an iterable.
First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG"
and arrival airport code as "LAX".
Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR" and arrival airport code as "CDG" .
Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "MIA" and arrival airport code as "LAX".
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightRepositoryTest {

    @Autowired
    private FlightRepository flightRepository;

    @Value("${local.server.port}")
    private int port;

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_NoResults() {
        String departureDate = "2023-08-01";
        String departureAirportCode = "CDG";
        String arrivalAirportCode = "LAX";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertFalse(flights.iterator().hasNext());
    }

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_SingleResult() {
        String departureDate = "2023-08-02";
        String departureAirportCode = "LHR";
        String arrivalAirportCode = "CDG";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertTrue(flights.iterator().hasNext());
        Assert.assertEquals(1, flights.spliterator().getExactSizeIfKnown());
    }

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_MultipleResults() {
        String departureDate = "2023-08-01";
        String departureAirportCode = "MIA";
        String arrivalAirportCode = "LAX";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertTrue(flights.iterator().hasNext());
        Assert.assertEquals(2, flights.spliterator().getExactSizeIfKnown());
    }
}
```