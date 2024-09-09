---
title : "Unit Test the service class"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---


- Now you will unit test the FlightDetailsService class using Junit. These test cases will validate the flow from service to the database (no mocking).
- Open FlightDetailsServiceTest.java under the test/java/com.airlines.catalog.test/ folder and add the following imports

![image.png](/images/module_1/unit_test_service/image.png)

- Create the class to test the FlightDetailsService
- First Test scenario: No flights found for the departure date, source and destination city.

{{< callout type="info" title="Prompt" >}}

First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG" and arrival airport code as "LAX", flightresultsRepository and airportresultsRepository as parameters. Assert count as 0

{{< /callout >}}

- Second Test scenario: Only one flight found for the departure date, source and destination city.

{{< callout type="info" title="Prompt" >}}

Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR", arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 1

{{< /callout >}}

- Third Test scenario: Two flights found for the departure date, source and destination city.

{{< callout type="info" title="Prompt" >}}

Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "LHR", arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 2

{{< /callout >}}

- Click on the arrow to the left of FlightDetailsServiceTest class declaration and choose Run FlightDetailsServiceTest. It will run all the test cases in the class. If any test fails, check the spring boot errors in the IDE. If the repository unit test cases were successful earlier, then check the assertion code. Verify if test data passed to the findFlights method matches with the test data in airport and flight tables.

![image.png](/images/module_1/unit_test_service/image_1.png)

- Complete code for FlightDetailsServiceTest class

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.dto.FlightDetails;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.repository.AirportRepository;
import com.airlines.catalog.service.FlightDetailsService;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.ArrayList;
import java.util.List;

/*
Create FlightDetailsServiceTest class to test the FlightDetailsService using
web environment with random port. Test cases for testing findFlights method in this class which  returns
list of flightDetails.
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightDetailsServiceTest {

    @Autowired
    FlightDetailsService flightDetailsService;

    @Autowired
    FlightRepository flightRepository;

    @Autowired
    AirportRepository airportRepository;

    @Value("${server.port}")
    private int port;

/*
First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG"
and arrival airport code as "LAX", flightresultsRepository and airportresultsRepository as parameters. Assert count as 0
*/
    @Test
    public void findFlightsTest() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-01", "CDG", "LAX", flightRepository, airportRepository);
        Assert.assertEquals(0, flightDetailsList.size());
    }
    /*
Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR",
 arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 1
 */
    @Test
    public void findFlightsTest2() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-02", "LHR", "CDG", flightRepository, airportRepository);
        Assert.assertEquals(1, flightDetailsList.size());
    }
    /*
Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "LHR",
arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 2
*/
    @Test
    public void findFlightsTest3() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-01", "LHR", "CDG", flightRepository, airportRepository);
        Assert.assertEquals(2, flightDetailsList.size());
    }
}
```