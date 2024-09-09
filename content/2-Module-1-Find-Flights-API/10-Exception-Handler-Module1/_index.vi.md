---
title : "Xây dựng Exception Handler để xử lý các Authentication Exceptions"
date :  "`r Sys.Date()`" 
weight : 10
chapter : false
pre : " <b> 2.10 </b> "
---

- Bây giờ chúng ta sẽ xây dựng trình xử lý ngoại lệ (Exception Handler) để xử lý các lỗi xác thực Cognito và xác định thông báo ứng dụng tùy chỉnh cho một số trường hợp ngoại lệ thường gặp.
- Mở **AuthenticationException.java** trong thư mục **src/main/java/com.airlines.catalog/Exception** và thêm các import sau:

![image.png](/images/module_1/exception_handler/image.png)

- Sử dụng prompt bên dưới để tạo lớp **AuthenticationException** sẽ xử lý các ngoại lệ liên quan đến xác thực JWT token.

{{< callout type="info" title="Prompt" >}}

Create a public class AuthenticationException that extends Runtime Exception with member variable response entity and constructor with Exception as input parameter.
Check for different types of JWT exceptions
Store the message in member variable response entity

{{< /callout >}}

![image.png](/images/module_1/exception_handler/image_1.png)

- Xác minh xem tất cả các loại ngoại lệ bắt buộc có có được xử lý hay không - **malformed URL, JWT decode, invalid claims and expired token**.

- Code hoàn chỉnh cho lớp **AuthenticationException**

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
