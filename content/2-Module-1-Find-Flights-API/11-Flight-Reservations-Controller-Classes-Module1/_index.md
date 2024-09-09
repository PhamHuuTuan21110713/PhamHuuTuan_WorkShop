---
title : "Create the FlightReservation Controller class"
date :  "`r Sys.Date()`" 
weight : 11
chapter : false
pre : " <b> 2.11 </b> "
---


- You will build the controller class for exposing the "getFlightDetails" API Amazon Q Developer prompts to build the API Controller:
- Open the FlightReservation.java under the src/main/java/com.airlines.catalog/controller folder. Add the below imports and starter code for the class.

{{< callout type="info" title="Code" >}}

import java.util.List;<br>

import com.airlines.catalog.dto.FlightDetails;<br>

import com.airlines.catalog.exception.AuthenticationException;<br>

import com.airlines.catalog.repository.AirportRepository;<br>

import com.airlines.catalog.repository.FlightRepository;<br>

import com.airlines.catalog.service.FlightDetailsService;<br>

import com.auth0.jwt.JWT;<br>

import com.auth0.jwt.JWTVerifier;<br>

import com.auth0.jwt.algorithms.Algorithm;<br>

import com.auth0.jwt.interfaces.DecodedJWT;<br>

import org.springframework.beans.factory.annotation.Autowired;<br>

import org.springframework.beans.factory.annotation.Value;<br>

import org.springframework.http.HttpStatus;<br>

import org.springframework.http.ResponseEntity;<br>

import org.springframework.web.bind.annotation.GetMapping;<br>

import org.springframework.web.bind.annotation.RequestHeader;<br>

import org.springframework.web.bind.annotation.RequestParam;<br>

import org.springframework.web.bind.annotation.RestController;<br>

@RestController<br>

public class FlightReservation {<br>

@Autowired<br>

FlightRepository flightresultsRepository;<br>

@Autowired<br>

AirportRepository airportresultsRepository;<br>

@Autowired<br>

FlightDetailsService FlightDetailsService;<br>

@Value("${cognito.userpool.id}")<br>

private String cognitoUserPoolId;<br>

@Value("${aws.region}")<br>

private String awsRegion;<br>

@Value("${sns.arn}")<br>

private String snsTopicArn;<br>

}

---

{{< /callout >}}

![image.png](/images/module_1/reservation_controller/image.png)

- Under the FlightReservation class, use the following prompt to create the verifyToken method. This method will use the AuthenticationException handler we created earlier.

{{< callout type="info" title="Prompt" >}}

Create a private method verifyToken to verify JWT token with input parameters asCognito user pool id, AWS region and token string. Function returns a Boolean. Construct the Cognito well known url and then verify the token using RSA Algorithm. Get the public key from AwsCognitoKeyProvider. Catch all Exception throw new authenticationException

{{< /callout >}}

![image.png](/images/module_1/reservation_controller/image_1.png)

- If there are any errors, select the whole method, right click and choose Send to Amazon Q and then Fix Code. This well give you the fixed code in the chat window which you can copy and then replace the function generated earlier. 
- Use the below prompt to create getFlightDetails controller which will expose REST API to find the flights.

{{< callout type="info" title="Prompt" >}}

Create a rest controller getFlightDetails to get flight details with HTTP GET Method, /flight path and request parameters as departure date, departure airport code and arrival airport code JWT Token in the Authorization Header.
Rest controller returns a ResponseEntity of string.

{{< /callout >}}

![image.png](/images/module_1/reservation_controller/image_2.png)

- Inside the getFlightDetails method use the below prompt to implement the API logic
{{< callout type="info" title="Prompt" >}}
    /* Call verifyToken method, catch authentication exception and return the responseEntity
    if token is valid and call the findFlights method in FlightDetailsService class
    with input parameters  departure date, departure airport code,
    arrival airport code, flightResultsRepository and airportResultsRepository.
    If flights are found return the list of flights otherwise return "No flights found".
    If authentication failed the return "Authentication failed" and HTTP status of forbidden
    */
{{< /callout >}}
- Current complete code for the **FlightReservation** class (we will continue to update this class in **Module 2** )

```java
    package com.airlines.catalog.controller;

import java.util.List;

import com.airlines.catalog.dto.FlightDetails;
import com.airlines.catalog.exception.AuthenticationException;
import com.airlines.catalog.repository.AirportRepository;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.service.FlightDetailsService;
import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.interfaces.DecodedJWT;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class FlightReservation {
    @Autowired
    FlightRepository flightresultsRepository;
    @Autowired
    AirportRepository airportresultsRepository;
    @Autowired
    FlightDetailsService FlightDetailsService;

    @Value("${cognito.userpool.id}")
    private String cognitoUserPoolId;
    @Value("${aws.region}")
    private String awsRegion;
    @Value("${sns.arn}")
    private String snsTopicArn;

    /* Create a private method verifyToken to verify JWT token with input parameters as
    Cognito user pool id, AWS region and token string. Function returns a Boolean.
    Construct the Cognito well known url and then verify the token using RSA Algorithm.
    catch all Exception throw new authenticationException.*/
    private Boolean verifyToken(String cognitoUserPoolId, String awsRegion, String token) throws AuthenticationException {
        try {
            String cognitoWellKnownUrl = "https://cognito-idp." + awsRegion + ".amazonaws.com/" + cognitoUserPoolId + "/.well-known/jwks.json";
            Algorithm algorithm = Algorithm.RSA256(new AwsCognitoRSAKeyProvider(cognitoWellKnownUrl));
            JWTVerifier verifier = JWT.require(algorithm).build();
            DecodedJWT decodedJWT = verifier.verify(token);
            return true;
        } catch (Exception e) {
            throw new AuthenticationException(e);
        }
    }

    /* Create a rest controller getFlightDetails to get flight details with HTTP GET Method ,
   /flight path and request parameters as departure date, departure airport code and arrival airport code
   JWT Token in the Authorization Header.
   Rest controller returns a ResponseEntity of string.
   */
    @GetMapping("/flight")
    public ResponseEntity<String> getFlightDetails(@RequestParam("departureDate") String departureDate,
                                                   @RequestParam("departureAirportCode") String departureAirportCode,
                                                   @RequestParam("arrivalAirportCode") String arrivalAirportCode,
                                                   @RequestHeader("Authorization") String authorization) throws AuthenticationException {
        /* Call verifyToken method, catch authentication exception and return the responseEntity
    if token is valid and call the findFlights method in FlightDetailsService class
    with input parameters  departure date, departure airport code,
    arrival airport code, flightResultsRepository and airportResultsRepository.
    If flights are found return the list of flights otherwise return "No flights found".
    If authentication failed the return "Authentication failed" and HTTP status of forbidden
    */
        try {
            if (verifyToken(cognitoUserPoolId, awsRegion, authorization)) {
                List<FlightDetails> flights = FlightDetailsService.findFlights(departureDate, departureAirportCode, arrivalAirportCode, flightresultsRepository, airportresultsRepository);
                if (flights.size() > 0) {
                    return new ResponseEntity<>(flights.toString(), HttpStatus.OK);
                } else {
                    return new ResponseEntity<>("No flights found", HttpStatus.OK);
                }
            } else {
                return new ResponseEntity<>("Authentication failed", HttpStatus.FORBIDDEN);
            }
        }
        catch (AuthenticationException e) {
            return new ResponseEntity<>(e.getResponseEntity().getBody().toString(), HttpStatus.UNAUTHORIZED);
        }
        
    }

}
```