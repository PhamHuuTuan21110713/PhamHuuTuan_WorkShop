---
title : "Module 1 - Find Flights API"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Find Flights API Overview

- In this module you will build an API to find flights based on departure date, departure airport code and arrival airport code. This API returns a list of flights with details about the flights, seats and price information. API calls are secured with a JWT token. User logs into Cognito Hosted UI to get the JWT Token and passes the token in the Authorization HTTP header. API verifies the JWT token and then allows the requested action to be performed. API interacts with flight and airport tables in RDS MySQL database to get the results. RDS credentials are stored securely in AWS Secret Manager.
- API returns the following fields:
    - Flight Id, Departure, Departure, Departure AirportCode, Departure Airport Name, departure Airport City, Departure Airport Locale Arrival Airport Code, Arrival Airport Name, Arrival Airport City, Arrival Airport Locale, Arrival Date, Arrival Time, Ticket Price Ticket Currency, Flight Number, Flight Duration; Seat Available
- Database for this module has 2 tables:
    - Airport table - stores the details of the Airport with Airport Code as primary key and associated airport details.
    - Flight table - stores the inventory of flights with their schedule, capacity and seat availability.
- Class Design for Module 1
  ![image](/images/module_1/sodolop.png?width=50pc&classes=shadow)
  

#### Content

1. [Setup the database tables for Module 1](1-table-database-module1)
2. [Build the model classes](2-model-classes-module1)
3. [Create Data Transfer Object (DTO)](3-dto-module1)
4. [Build JPA Repository Interface](4-jpa-repository-interface-module1)
5. [Data Source configuration](5-config-datasource-module1)
6. [Build the service class](6-service-classes-module1)
7. [Unit Testing of the repository classes](7-unit-test-repository-classes-module1)
8. [Unit Test the service class](8-unit-test-service-classes-module1)
9. [Get Cognito public keys](9-get-pkeys-module1)
10. [Build Exception Handler to handle Authentication Exceptions](10-exception-handler-module1)
11. [Create the FlightReservation Controller class](11-flight-reservations-controller-classes-module1)
12. [Unit Test Find Flights API](12-unit-test-find-flights-api)
