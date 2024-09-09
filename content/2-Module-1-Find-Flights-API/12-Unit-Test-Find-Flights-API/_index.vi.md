---
title : "Unit test API tìm chuyến bay"
date :  "`r Sys.Date()`" 
weight : 12
chapter : false
pre : " <b> 2.12 </b> "
---

- Unit Test của phương thức **getFlightDetailsApiTest** trong Lớp **Controller**. API sẽ được kiểm tra tích hợp từ đầu đến cuối.
- Mở **getFlightDetailsApiTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau

![image.png](/images/module_1/unit_test_find_api/image.png)

- Tạo lớp **bookingApiTest** với prompt

{{< callout type="info" title="Prompt" >}}

Create a bookingApiTest class to test the FlightReservation class /flight endpoint  using web environment with random port.

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_1.png)

- Kịch bản thử nghiệm 1: JWT token không hợp lệ. Đặt Authorization Header là " Bearer invalidToken ". Dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

Create a method for token invalid test case.
Create the HTTP headers object and pass the jwtToken
url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
make a get request
Assert "Invalid Token" is returned in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_2.png)

- Đối với tất cả các kịch bản thử nghiệm bên dưới, hãy sử dụng token mà bạn đã tạo trước đó từ Cognito trong quá trình Đăng ký. Nếu bạn gặp lỗi hết hạn token, hãy làm theo các bước "Đăng nhập" trong chương "Thiết lập Cognito" để nhận token mới.
- Kịch bản thử nghiệm 2: JWT token hợp lệ và các chuyến bay có sẵn cho dữ liệu thử nghiệm được cung cấp.
{{< callout type="info" title="Prompt" >}}

Create a method for token valid test case.
create the HTTP headers object and pass the jwtToken. URL is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX make a get request
Assert "FlightDetails" is contained in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_3.png)

- Kịch bản thử nghiệm 3: JWT token hợp lệ và các chuyến bay không có sẵn cho dữ liệu thử nghiệm được cung cấp. Dùng prompt

{{< callout type="info" title="Prompt" >}}

Create a method for no flights available.
create the HTTP headers object and pass the jwtToken.
url is /flight end point with departureDate=2023-08-01, departureAirportCode=CDG, arrivalAirportCode=LHR,
make a get request
Assert that response contains "No flights found"

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_4.png)

- Kịch bản thử nghiệm 4: JWT token hợp lệ và chỉ có một chuyến bay cho dữ liệu thử nghiệm được cung cấp.

{{< callout type="info" title="Prompt" >}}

Create a method for single flight available test case.
Create the HTTP headers object and pass the jwtToken.
url /flight end point with departureDate=2023-08-02, departureAirportCode=LHR, arrivalAirportCode=CDG,
make a get request
Assert "FlightDetails" is contained only once in response

{{< /callout >}}

![image.png](/images/module_1/unit_test_find_api/image_5.png)

- Tiếp theo, tôi sẽ lấy token còn hạn sử dụng bằng cách đăng nhập qua Cognito, sau đó tôi dùng token đó thay thế vào code, sau đó chuột phải vào **getFlightDetailsApiTest**.java, cuối cùng chọn Run **getFlightDetailsApiTest** và chờ kết quả
- Tôi đã lấy token và tiến hành test, và nó đã pass cả 4 test case

![image.png](/images/module_1/unit_test_find_api/image_6.png)

- Code hoàn chỉnh cho lớp **GetFlightDetailsApiTest**

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;

import static org.assertj.core.api.Assertions.assertThat;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

/*
Create a bookingApiTest class to test the FlightReservation class /flight endpoint  using
web environment with random port.
*/

@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class GetFlightDetailsApiTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Value("${server.port}")
    private int port;

    /*
Create a method for token invalid test case.
Create the HTTP headers object and pass the jwtToken
url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
make a get request
Assert "Invalid Token" is returned in response
*/
    @Test
    public void testTokenInvalid() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer invalidToken");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=MIA&arrivalAirportCode=LAX", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);
        assertThat(response.getBody()).contains("Invalid Token");
    }

    /*
     Create a method for token valid test case.
     Create the HTTP headers object and pass the jwtToken.
     url is /flight end point with query parameters departureDate=2023-08-01, departureAirportCode=MIA, arrivalAirportCode=LAX
     make a get request
     Assert "FlightDetails" is contained in response
     */
    @Test
    public void testTokenValid() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=MIA&arrivalAirportCode=LAX", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("FlightDetails");
    }

    /*
   Create a method for no flights available.
   Create the HTTP headers object and pass the jwtToken.
   url is /flight end point with departureDate=2023-08-01, departureAirportCode=CDG, arrivalAirportCode=LHR,
   make a get request
   Assert that response contains "No flights found"
   */
    @Test
    public void testNoFlightsAvailable() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-01&departureAirportCode=CDG&arrivalAirportCode=LHR", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("No flights found");

    }

    /*
   Create a method for single flight available test case.
   Create the HTTP headers object and pass the jwtToken.
   url /flight end point with departureDate=2023-08-02, departureAirportCode=LHR, arrivalAirportCode=CDG,
   make a get request
   Assert "FlightDetails" is contained only once in response
   */
    @Test
    public void testSingleFlightAvailable() {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "<Get Your Token from Cognito>");
        HttpEntity<String> entity = new HttpEntity<>(headers);
        ResponseEntity<String> response = restTemplate.exchange("/flight?departureDate=2023-08-02&departureAirportCode=LHR&arrivalAirportCode=CDG", HttpMethod.GET, entity, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).contains("FlightDetails");
    }
}
```