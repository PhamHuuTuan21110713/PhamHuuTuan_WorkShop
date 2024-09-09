---
title : "Unit Testing of the reserve Flight API"
date :  "`r Sys.Date()`" 
weight : 10
chapter : false
pre : " <b> 3.10 </b> "
---

- Now you will create the Junit test scripts amd unit test the Reserve Flight API. API will be integrated with the backend and tested via Postman.
- Open the ReserveFlightApiTest.java in the folder test/java/com.airlines.catalog.test/ and add the following imports.

{{< callout type="info" title="Code" >}}

import com.airlines.catalog.FlightBookingApplication;<br>
import com.airlines.catalog.dto.ReservationDetails;<br>
import org.junit.jupiter.api.Test;<br>
import org.springframework.beans.factory.annotation.Autowired;<br>
import org.springframework.beans.factory.annotation.Value;<br>
import org.springframework.boot.test.context.SpringBootTest;<br>
import org.springframework.boot.test.web.client.TestRestTemplate;<br>
import org.springframework.http.HttpEntity;<br>
import org.springframework.http.HttpHeaders;<br>
import org.springframework.http.HttpStatus;<br>
import org.springframework.http.ResponseEntity;<br>
import static org.assertj.core.api.Assertions.assertThat;<br>

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image.png)

- We use the prompt below to create the **ReserveFlightApiTest** class, **and delete the methods created by Amazon Q chat**.

{{< callout type="info" title="Prompt" >}}

Create a ReserveFlightApiTest class to test the bookingApi using SpringBootTest

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_1.png)

- Next we create the test cases.
- Test Scenario 1 - JWT Token is invalid. Set the Authorization Header as "Bearer invalidToken". Ensure that valid email Id format, contactNumber format and age have valid values. Otherwise the test will fail as data validation happens before the token verification.

{{< callout type="info" title="Prompt" >}}

Negative scenaio: 1, Invalid jwtToken
create the reservationDetails object   with fields firstName, lastName, gender, age,
flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
and contactEmail attributes
Create the HTTP headers object and pass the jwtToken.
Call /reserve end point using post method  pass reservationDetails as request body
Assert that the response message is "Invalid Token"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_2.png)

- For all the test scenarios below, use the token that you generated earlier from Cognito during Signup process. If you get token expired error then follow the "Sign-in" steps in "Set up Cognito" to get new token.
- Test Scenario 2 - Contact number is invalid and JWT Token is valid.

{{< callout type="info" title="Prompt" >}}

Negative scenaio:  Invalid Contact number
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, invalid contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that the response message contains "Contact Number should be valid phone number"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_3.png)

- Test Scenario 3 - JWT Token and all input parameters are valid
{{< callout type="info" title="Prompt" >}}

Positive scenario 1:
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that http status OK
   and  assert that the response message contains "reservation made successfully"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_4.png)

- Now we will log in to get the token and run this file, if it passes all then it is ok.

![image.png](/images/module_2/unit_test_reservation_api/image_5.png)
- I ran it and passed all 3 test cases.

![image.png](/images/module_2/unit_test_reservation_api/image_6.png)

- Complete code for bookingAPIReserveTest class

```java
package com.airlines.catalog.test;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.dto.ReservationDetails;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import static org.assertj.core.api.Assertions.assertThat;

/*
Create a BookingAPIReserveTest class to test the bookingApi using
web environment with random port.
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ReserveFlightApiTest {

    String myToken = "Put Your Token Here ahihi :3";
    @Value("${local.server.port}")
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    /* Negative scenaio: 1, Invalid jwtToken
   create the reservationDetails object   with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   Create the HTTP headers object and pass the jwtToken.
   Call /reserve end point using post method  pass reservationDetails as request body
   Assert that the response message is "Invalid Token"
 */
    @Test
    public void testReserveFlightWithInvalidToken() {
        // Create a ReservationDetails object with the necessary data
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("John");
        reservationDetails.setLastName("Doe");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("Economy");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("1234567890");
        reservationDetails.setContactEmail("john.doe@example.com");

        // Create an HTTP entity with the reservation details and an invalid JWT token
        HttpHeaders headers = new HttpHeaders();
        headers.setBearerAuth("invalid_token");
        HttpEntity<ReservationDetails> requestEntity = new HttpEntity<>(reservationDetails, headers);

        // Send a POST request to the booking API endpoint
        ResponseEntity<String> responseEntity = restTemplate.postForEntity("/reserve", requestEntity, String.class);

        // Assert the response status code and message
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);
        assertThat(responseEntity.getBody()).isEqualTo("Invalid Token");
    }

    /* Negative scenaio:  Invalid Contact number
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, invalid contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that the response message contains "Contact number must be a 10-digit number"
 */
    @Test
    public void testInvalidContactNumber() {
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("XXXX");
        reservationDetails.setLastName("XXX");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("First Class");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("1234");
        reservationDetails.setContactEmail("XX@gmail.com");

        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization",myToken);
        HttpEntity<ReservationDetails> request = new HttpEntity<>(reservationDetails, headers);

        ResponseEntity<String> response = restTemplate.postForEntity("/reserve", request, String.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.BAD_REQUEST);
        assertThat(response.getBody()).contains("Contact number must be a 10-digit number");

    }

    /* Positive scenario 1:
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that http status OK
   and  assert that the response message contains "reservation made successfully"
   */
    @Test
    public void testPositiveScenario() {
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("XXXX");
        reservationDetails.setLastName("XXX");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("First Class");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("0928895717");
        reservationDetails.setContactEmail("phamhuutuanaws@gmail.com");

        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization",myToken);
        HttpEntity<ReservationDetails> request = new HttpEntity<>(reservationDetails, headers);
        ResponseEntity<String> response = restTemplate.postForEntity("/reserve", request, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    }
}
```