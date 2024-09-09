---
title : "Xử lý các Business Exceptions"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

- Tiếp theo, chúng sẽ tạo các lớp xử lý ngoại lệ để xử lý business exception: Dữ liệu đầu vào không hợp lệ đối với API đặt chỗ, Id chuyến bay không hợp lệ và không còn chỗ trống trên chuyến bay.
- Mở tệp **GlobalExceptionHandler.java** trong thư mục **src/main/java/com.airlines.catalog/Exception**, từ Amazon Q Chat Panel trong IDE của bạn, hãy sử dụng prompt bên dưới để tạo lớp **GlobalExceptionHandler** hoàn chỉnh. Sao chép mã đó vào **GlobalExceptionHandler.java**. Lớp này sẽ được sử dụng để xử lý các trường hợp ngoại lệ khi dữ liệu đầu vào không hợp lệ được cung cấp cho API **bookFlight**.

{{< callout type="info" title="Prompt" >}}
Create a rest controller advice GlobalExceptionHandler with handleValidationErrors method that gets all the errors and returns ResponseEntity with hash map   
{{< /callout >}}
![image.png](/images/module_2/bussiness_exception/image.png)

- Tiếp theo mở **FlightNotFoundException.java** trong thư mục **src/main/java/com.airlines.catalog/exception** và thêm các câu import sau.

![image.png](/images/module_2/bussiness_exception/image_1.png)

- Sử dụng prompt bên dưới để xử lý ngoại lệ khi cung cấp Id chuyến bay không hợp lệ cho ngoại lệ đặt chỗ

{{< callout type="info" title="Prompt" >}}

Create a public class FlightNotFoundException that extends Runtime Exception with member variable response entity.
Create constructor with flightId as input parameter
Exception message returned should be "No flights found for the flight id " and append flight id

{{< /callout >}}

![image.png](/images/module_2/bussiness_exception/image_2.png)

- Mở **RequestedSeatsNotAvailable.java** trong thư mục **src/main/java/com.airlines.catalog/Exception** và thêm các câu import sau.

![image.png](/images/module_2/bussiness_exception/image_3.png)

- Hãy sử dụng prompt bên dưới để xử lý trường hợp ngoại lệ khi không còn chỗ trống trên chuyến bay để đặt chỗ.

{{< callout type="info" title="Prompt" >}}

Create a public class RequestedSeatsNotAvailable that extends Runtime Exception with member variable response entity and returns an exception message e "Seats not available. Reservation could not be made"

{{< /callout >}}

![image.png](/images/module_2/bussiness_exception/image_4.png)

- Code hoàn chỉnh cho lớp GlobalExceptionHandler
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

- Code hoàn chỉnh cho lớp **FlightNotFoundException**

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

- Code hoàn chỉnh cho lớp **RequestedSeatsNotAvailable**

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