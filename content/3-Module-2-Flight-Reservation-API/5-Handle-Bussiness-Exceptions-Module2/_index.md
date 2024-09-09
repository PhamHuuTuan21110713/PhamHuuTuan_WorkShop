---
title : "Handle Business Exceptions"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

- Next you will create exception handler classes to handle following business exception: Invalid data inputs to the reservation API, invalid flight Id and no seats available in the flight.
- Open the GlobalExceptionHandler.java file under the src/main/java/com.airlines.catalog/exception folder, from the Amazon Q Chat Panel in your IDE, use the below prompt to create the complete GlobalExceptionHandler class. Click on Copy button below the code panel and paste the code into the GlobalExceptionHandler.java file. This class will be used to handle the exceptions when invalid inputs are provided to bookFlight API.

{{< callout type="info" title="Prompt" >}}
Create a rest controller advice GlobalExceptionHandler with handleValidationErrors method that gets all the errors and returns ResponseEntity with hash map   
{{< /callout >}}
![image.png](/images/module_2/bussiness_exception/image.png)

- Open the FlightNotFoundException.java under the src/main/java/com.airlines.catalog/exception folder and add the following import statements.

![image.png](/images/module_2/bussiness_exception/image_1.png)

- Use the prompts to below to handle exception when invalid flight Id provided for reservation exception

{{< callout type="info" title="Prompt" >}}

Create a public class FlightNotFoundException that extends Runtime Exception with member variable response entity.
Create constructor with flightId as input parameter
Exception message returned should be "No flights found for the flight id " and append flight id

{{< /callout >}}

![image.png](/images/module_2/bussiness_exception/image_2.png)

- Open the RequestedSeatsNotAvailable.java under the src/main/java/com.airlines.catalog/exception folder and add the following import statements.

![image.png](/images/module_2/bussiness_exception/image_3.png)

- Use the prompts to below to handle exception when no seats available in the flight for reservation.

{{< callout type="info" title="Prompt" >}}

Create a public class RequestedSeatsNotAvailable that extends Runtime Exception with member variable response entity and returns an exception message e "Seats not available. Reservation could not be made"

{{< /callout >}}

![image.png](/images/module_2/bussiness_exception/image_4.png)

- Complete code for GlobalExceptionHandler class
```java
package com.airlines.catalog.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import java.util.HashMap;
import java.util.Map;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationErrors(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach(error -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }
}
```

- Complete code for FlightNotFoundException class

```java
package com.airlines.catalog.exception;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import lombok.Getter;

/*
Create a public class FlightNotFoundException that extends Runtime Exception with member variable
response entity.
Create constructor with flightId as input parameter
Exception message returned should be "No flights found for the flight id " and append flight id
*/
@Getter
public class FlightNotFoundException extends RuntimeException {
    private ResponseEntity<String> responseEntity;

    public FlightNotFoundException(String flightId) {
        responseEntity = new ResponseEntity<>("No flights found for the flight id " + flightId, HttpStatus.BAD_REQUEST);
    }
}
```

- Complete code for RequestedSeatsNotAvailable class

```java
package com.airlines.catalog.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import lombok.Getter;
/*
Create a public class RequestedSeatsNotAvailable that extends Runtime Exception
with member variable response entity and returns an
exception message e "Seats not available. Reservation could not be made"
*/
@Getter
public class RequestedSeatsNotAvailable extends RuntimeException {
    private ResponseEntity<String> responseEntity;

    public RequestedSeatsNotAvailable() {
        responseEntity = new ResponseEntity<>("Seats not available. Reservation could not be made", HttpStatus.BAD_REQUEST);
    }
}
```