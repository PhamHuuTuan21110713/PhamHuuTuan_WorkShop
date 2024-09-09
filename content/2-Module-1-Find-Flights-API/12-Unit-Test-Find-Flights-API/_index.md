---
title : "Unit Test Find Flights API"
date :  "`r Sys.Date()`" 
weight : 12
chapter : false
pre : " <b> 2.12 </b> "
---

- Unit Test of getFlightDetailsApiTest method in the Controller Class. The API will be integration tested end to end.
- Open getFlightDetailsApiTest.java under the test/java/com.airlines.catalog.test/ folder and add the following imports

![image.png](/images/module_1/unit_test_find_api/image.png)

- Create bookingApiTest class

{{< callout type="info" title="Prompt" >}}

Create a bookingApiTest class to test the FlightReservation class /flight endpoint  using web environment with random port.

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_1.png)

- Test Scenario 1: JWT Token is invalid. Set the Authorization Header as "Bearer invalidToken".

{{< callout type="info" title="Prompt" >}}

Create a method for token invalid test case.
Create the HTTP headers object and pass the jwtToken
url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
make a get request
Assert "Invalid Token" is returned in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_2.png)

- For all the test scenarios below, use the token that you generated earlier from Cognito during Signup process. If you get token expired error then follow the "Sign-in" steps in "Set up Cognito" chapter to get new token.
- Test scenario 2: JWT Token is valid and the flights are available for the provided test data.
{{< callout type="info" title="Prompt" >}}

Create a method for token valid test case.
create the HTTP headers object and pass the jwtToken. URL is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX make a get request
Assert "FlightDetails" is contained in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_3.png)

- Test scenario 3: JWT Token is valid and the flights are not available for the provided test data.

{{< callout type="info" title="Prompt" >}}

Create a method for no flights available.
create the HTTP headers object and pass the jwtToken.
url is /flight end point with departureDate=2023-08-01, departureAirportCode=CDG, arrivalAirportCode=LHR,
make a get request
Assert that response contains "No flights found"

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_4.png)

- Test scenario 4: JWT Token is valid and only one flight is available for the provided test data.

{{< callout type="info" title="Prompt" >}}

Create a method for single flight available test case.
Create the HTTP headers object and pass the jwtToken.
url /flight end point with departureDate=2023-08-02, departureAirportCode=LHR, arrivalAirportCode=CDG,
make a get request
Assert "FlightDetails" is contained only once in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_5.png)

- Next, I will get the valid token by logging in via Cognito, then I use that token to replace it in the code, then right click on **getFlightDetailsApiTest**.java, finally select Run **getFlightDetailsApiTest** and wait for the result
- I took the token and ran a test, and it passed all 4 test cases.

![image.png](/images/module_1/unit_test_find_api/image_6.png)

- Complete code for getFlightDetailsApiTest class

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;

import static org.assertj.core.api.Assertions.assertThat;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

/*
Create a bookingApiTest class to test the FlightReservation class /flight endpoint  using
web environment with random port.
*/

@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class GetFlightDetailsApiTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Value("${server.port}")
    private int port;

    /*
Create a method for token invalid test case.
Create the HTTP headers object and pass the jwtToken
url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
make a get request
Assert "Invalid Token" is returned in response
*/
    @Test
    public void testTokenInvalid() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer invalidToken");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=MIA&arrivalAirportCode=LAX", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);
        assertThat(response.getBody()).contains("Invalid Token");
    }

    /*
     Create a method for token valid test case.
     Create the HTTP headers object and pass the jwtToken.
     url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
     make a get request
     Assert "FlightDetails" is contained in response
     */
    @Test
    public void testTokenValid() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=MIA&arrivalAirportCode=LAX", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("FlightDetails");
    }

    /*
   Create a method for no flights available.
   Create the HTTP headers object and pass the jwtToken.
   url is /flight end point with departureDate=2023-08-01, departureAirportCode=CDG, arrivalAirportCode=LHR,
   make a get request
   Assert that response contains "No flights found"
   */
    @Test
    public void testNoFlightsAvailable() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=CDG&arrivalAirportCode=LHR", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("No flights found");

    }

    /*
   Create a method for single flight available test case.
   Create the HTTP headers object and pass the jwtToken.
   url /flight end point with departureDate=2023-08-02, departureAirportCode=LHR, arrivalAirportCode=CDG,
   make a get request
   Assert "FlightDetails" is contained only once in response
   */
    @Test
    public void testSingleFlightAvailable() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-02&departureAirportCode=LHR&arrivalAirportCode=CDG", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("FlightDetails");
    }
}
```