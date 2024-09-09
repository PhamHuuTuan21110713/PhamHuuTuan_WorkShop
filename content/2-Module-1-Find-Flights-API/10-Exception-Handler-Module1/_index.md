---
title : "Build Exception Handler to handle Authentication Exceptions"
date :  "`r Sys.Date()`" 
weight : 10
chapter : false
pre : " <b> 2.10 </b> "
---

- You will now build the exception handler to handle Cognito authentication errors and define custom application messages for few commonly encountered exceptions.
- Open the AuthenticationException.java under the src/main/java/com.airlines.catalog/exception folder and add the following imports:

![image.png](/images/module_1/exception_handler/image.png)

- Use the prompt below to create the AuthenticationException class that will handle the exceptions related to JWT token validation.

{{< callout type="info" title="Prompt" >}}

Create a public class AuthenticationException that extends Runtime Exception with member variable response entity and constructor with Exception as input parameter.
Check for different types of JWT exceptions
Store the message in member variable response entity

{{< /callout >}}

![image.png](/images/module_1/exception_handler/image_1.png)

- Verify if all exception required exception types are handled - malformed URL, JWT decode, invalid claims and expired token.

- Complete code for AuthenticationException class

```java
package com.airlines.catalog.exception;

import com.auth0.jwt.exceptions.InvalidClaimException;
import com.auth0.jwt.exceptions.JWTDecodeException;
import com.auth0.jwt.exceptions.TokenExpiredException;
import java.net.MalformedURLException;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import lombok.Getter;
/*Create a public class AuthenticationException that extends Runtime Exception with member variable
response entity and constructor with Exception as input parameter.
Check for different types of JWT exceptions
Store the message in member variable response entity.
*/
@Getter
public class AuthenticationException extends RuntimeException {

    private ResponseEntity responseEntity;

    public AuthenticationException(Exception e) {
        if (e instanceof JWTDecodeException) {
            responseEntity = new ResponseEntity<>("Invalid Token", HttpStatus.UNAUTHORIZED);
        } else if (e instanceof TokenExpiredException) {
            responseEntity = new ResponseEntity<>("Token Expired", HttpStatus.UNAUTHORIZED);
        } else if (e instanceof InvalidClaimException) {
            responseEntity = new ResponseEntity<>("Invalid Claim", HttpStatus.UNAUTHORIZED);
        } else if (e instanceof MalformedURLException) {
            responseEntity = new ResponseEntity<>("Invalid URL", HttpStatus.UNAUTHORIZED);
        }
    }
}
```
