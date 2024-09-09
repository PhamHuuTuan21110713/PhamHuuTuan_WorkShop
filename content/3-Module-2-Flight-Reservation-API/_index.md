---
title : "Module 2 - Flight Reservation API"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Reserve Flight API overview

- In this Module you will build a Reserve Flight API that will provide the functionality to book a flight. API will take passenger details, reservation details and details of the flight to book as input. Flight Id returned from the findFlights API call will provided as input to this API. API calls are secured with a JWT token. User will log into Cognito Hosted UI to get the JWT Token and pass the token in the Authorization HTTP header. API verifies the token before saving data to passenger and reservation tables in the database. API will perform following functions:
    - Validation of reservation details provided in the request such as Not Null validation, Email format validation and Phone Number format validation.
    - Store the data after validation into passenger and reservation tables. Send out a booking confirmation notification with the reservation details and booking reference number to SNS Topic. This entire step needs to happen in an atomic transaction.
    - User gets the booking reference number via email and also in the API response.

#### Class design for module 2

![image.png](/images/module_2/class_design.png?classes=shadow)

### Content

1. [Create Tables and load data](1-table-database-module2)
2. [Build the model layer](2-model-classes-module2)
3. [Build the JPA Repository](3-jpa-repository-module2)
4. [Unit test the Repository classes](4-unit-test-repository-classes-module2)
5. [Handle Business Exceptions](5-handle-business-exceptions-module2)
6. [Build the service class](6-service-classes-module2)
7. [Unit Test FlightBooking service class](7-unit-test-flightbooking-service-classes)
8. [Create DTO Reservation Details](8-dto-reservation-details)
9. [Create the book flight API Controller](9-book-flight-api-controller)
10. [Unit Testing of the reserve Flight API](10-unit-test-reserve-flight-api)