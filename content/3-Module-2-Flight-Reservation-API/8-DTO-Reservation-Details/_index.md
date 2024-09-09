---
title : "Create DTO Reservation Details"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 3.8 </b> "
---

- Now you will create a ReservationDetails DTO class that will be used to validate all and capture the input parameters provided to the bookFlight API.
- Open ReservationDetails.java under the src/main/java/com.airlines.catalog/dto folder.
- From the Amazon Q Chat Panel in your IDE, use the below prompt to create the complete ReservationDetails class. Click on Copy button below the code panel and paste the code into the ReservationDetails.java file.
{{< callout type="info" title="Prompt" >}}
Create a class ReservationDetails with attributes
firstName not blank, lastName not blank,
gender, age between 1 and 120, flightId as int, travelClass not blank,
ticketPrice as double and not blank,
currencyCode exactly 3 characters, reservationStatus, paymentStatus,
paymentMode, ContactEmail should be valid email, contactNumber should be valid phone number
{{< /callout >}}
![image.png](/images/module_2/dto/image.png)

- Complete code for reservationDetails class

```java
    package com.airlines.catalog.dto;

    import lombok.Data;
    import javax.validation.constraints.Email;
    import javax.validation.constraints.Min;
    import javax.validation.constraints.Max;
    import javax.validation.constraints.NotBlank;
    import javax.validation.constraints.Pattern;
    import javax.validation.constraints.Positive;

    @Data
    public class ReservationDetails {

        @NotBlank(message = "First name cannot be blank")
        private String firstName;

        @NotBlank(message = "Last name cannot be blank")
        private String lastName;

        private String gender;
    
        @Min(value = 1, message = "Age should be between 1 and 120")
        @Max(value = 120, message = "Age should be between 1 and 120")
        private int age;

        @Positive(message = "Flight ID must be a positive integer")
        private int flightId;

        @NotBlank(message = "Travel class cannot be blank")
        private String travelClass;

        @Positive(message = "Ticket price must be a positive value")
        private double ticketPrice;

        @Pattern(regexp = "^[A-Z]{3}$", message = "Currency code must be exactly 3 uppercase letters")
        private String currencyCode;

        private String reservationStatus;
        private String paymentStatus;
        private String paymentMode;

        @Email(message = "Invalid email address")
        private String contactEmail;

        @Pattern(regexp = "^\\d{10}$", message = "Contact number must be a 10-digit number")
        private String contactNumber;
    }

```