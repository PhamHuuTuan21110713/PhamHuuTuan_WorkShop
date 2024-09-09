---
title : "Home"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Amazon Q Developer Workshop - Build enterprise Java application with Spring Boot

#### Overview
- This workshop aims to help us learn how to build an enterprise Java application in a microservices architecture using Spring Boot, with the help of Amazon Q Developer, an AI coding companion from Amazon. We will develop a modest microservices application to understand how to use Amazon Q Developer in real-world projects. We will not only create code but also integrate with various AWS services and test the application.

#### Technology used
- The application we are building is a flight booking application, and the application will use Microservices architecture, we will write code and use Intelij IDEA IDE. Also need knowledge of Java, Spring Boot, SQL and Junit.
- We will learn how to perform following development tasks using Amazon Q Developer:
    - Create Database Schema
    - Load Data into Tables
    - Implement the Business Logic for API
    - Integrate AWS Services
    - Create Unit Test Scripts for the Classes
#### Modules

- Our application consists of 2 Modules, including:
    1. **Find flights**
    - This API will get a list of flights based on query parameters departure date, departure city and arrival city. In this module you will use Amazon Q Developer to implement integration with Amazon Cognito, AWS Secrets Manager and Amazon RDS using Spring Data JPA.
    1. **Book flight**
    - This API will reserve the flight and send a confirmation email to the passenger with booking details. In this module you will use Amazon Q Developer to implement input data validation, data mapping, custom exception handling, transaction management and notification using Amazon SNS. Module 2 leverages some of the code built in Module 1. So it is recommended to complete Module 1 before proceeding to Module 2.
  
#### Architecture
![image.png](/images/Kientruc/image.png/)
#### Content

1. [Setup Development Environment](1-config-environment/)
2. [Module 1 - Find Flights API](2-module-1-find-flights-api/)
3. [Module 2 - Flight Reservation API](3-module-2-flight-reservation-api/)
4. [Test API with Postman](4-testing-api-with-postman/)
5. [Clean up](5-clean-up/)