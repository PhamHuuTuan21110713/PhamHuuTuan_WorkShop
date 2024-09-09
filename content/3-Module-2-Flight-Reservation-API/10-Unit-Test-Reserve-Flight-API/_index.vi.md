---
title : "Unit test cho  API đặt chỗ chuyến bay"
date :  "`r Sys.Date()`" 
weight : 10
chapter : false
pre : " <b> 3.10 </b> "
---

- Bây giờ chúng ta sẽ tạo tập lệnh test Junit và unit test API chuyến bay dự trữ. API sẽ được thử nghiệm thông qua Postman.
- Mở **ReserveFlightApiTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau

{{< callout type="info" title="Code" >}}

import com.airlines.catalog.FlightBookingApplication;<br>
import com.airlines.catalog.dto.ReservationDetails;<br>
import org.junit.jupiter.api.Test;<br>
import org.springframework.beans.factory.annotation.Autowired;<br>
import org.springframework.beans.factory.annotation.Value;<br>
import org.springframework.boot.test.context.SpringBootTest;<br>
import org.springframework.boot.test.web.client.TestRestTemplate;<br>
import org.springframework.http.HttpEntity;<br>
import org.springframework.http.HttpHeaders;<br>
import org.springframework.http.HttpStatus;<br>
import org.springframework.http.ResponseEntity;<br>
import static org.assertj.core.api.Assertions.assertThat;<br>

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image.png)

- Chúng ta dùng prompt dưới đây để tiến hành tạo lớp **ReserveFlightApiTest**, ****và xóa đi các method do Amazon Q chat tạo ra**.**

{{< callout type="info" title="Prompt" >}}

Create a ReserveFlightApiTest class to test the bookingApi using SpringBootTest

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_1.png)

- Tiếp theo chúng ta tạo các trường hợp thử nghiệm
- Kịch bản thử nghiệm 1 - Mã thông báo JWT không hợp lệ. Đặt Tiêu đề ủy quyền là "Mã thông báo không hợp lệ". Đảm bảo rằng định dạng Id email hợp lệ, định dạng Số liên lạc và độ tuổi có giá trị hợp lệ. Nếu không, quá trình kiểm tra sẽ thất bại do việc xác thực dữ liệu diễn ra trước khi xác minh mã thông báo.

{{< callout type="info" title="Prompt" >}}

Negative scenaio: 1, Invalid jwtToken
create the reservationDetails object   with fields firstName, lastName, gender, age,
flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
and contactEmail attributes
Create the HTTP headers object and pass the jwtToken.
Call /reserve end point using post method  pass reservationDetails as request body
Assert that the response message is "Invalid Token"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_2.png)

- Đối với tất cả các kịch bản thử nghiệm bên dưới, hãy sử dụng Token mà bạn đã tạo trước đó từ **Cognito** trong quá trình Đăng ký (**Sign-in**). Nếu chúng ta gặp lỗi hết hạn token, chúng ta chỉ cần đăng nhập và lấy lại token mới là được.
- Kịch bản thử nghiệm 2 - Số liên hệ không hợp lệ và JWT token hợp lệ, dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

Negative scenaio:  Invalid Contact number
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, invalid contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that the response message contains "Contact Number should be valid phone number"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_3.png)

- Kịch bản thử nghiệm 3 - JWT token và tất cả các tham số đầu vào đều hợp lệ.

{{< callout type="info" title="Prompt" >}}

Positive scenario 1:
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that http status OK
   and  assert that the response message contains "reservation made successfully"

{{< /callout >}}

![image.png](/images/module_2/unit_test_reservation_api/image_4.png)

- Bây giờ chúng ta sẽ đăng nhập để lấy token và tiến hành run file này, nếu pass qua hết thì được.

![image.png](/images/module_2/unit_test_reservation_api/image_5.png)
- Tôi tiến hành chạy và đã pass được cả 3 test case

![image.png](/images/module_2/unit_test_reservation_api/image_6.png)

- Code hoan chỉnh cho lớp **ReseveFlightApiTest**

```java
package com.airlines.catalog.test;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.dto.ReservationDetails;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import static org.assertj.core.api.Assertions.assertThat;

/*
Create a BookingAPIReserveTest class to test the bookingApi using
web environment with random port.
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class ReserveFlightApiTest {

    String myToken = "Put Your Token Here ahihi :3";
    @Value("${local.server.port}")
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    /* Negative scenaio: 1, Invalid jwtToken
   create the reservationDetails object   with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   Create the HTTP headers object and pass the jwtToken.
   Call /reserve end point using post method  pass reservationDetails as request body
   Assert that the response message is "Invalid Token"
 */
    @Test
    public void testReserveFlightWithInvalidToken() {
        // Create a ReservationDetails object with the necessary data
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("John");
        reservationDetails.setLastName("Doe");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("Economy");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("1234567890");
        reservationDetails.setContactEmail("john.doe@example.com");

        // Create an HTTP entity with the reservation details and an invalid JWT token
        HttpHeaders headers = new HttpHeaders();
        headers.setBearerAuth("invalid_token");
        HttpEntity<ReservationDetails> requestEntity = new HttpEntity<>(reservationDetails, headers);

        // Send a POST request to the booking API endpoint
        ResponseEntity<String> responseEntity = restTemplate.postForEntity("/reserve", requestEntity, String.class);

        // Assert the response status code and message
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.UNAUTHORIZED);
        assertThat(responseEntity.getBody()).isEqualTo("Invalid Token");
    }

    /* Negative scenaio:  Invalid Contact number
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, invalid contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that the response message contains "Contact number must be a 10-digit number"
 */
    @Test
    public void testInvalidContactNumber() {
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("XXXX");
        reservationDetails.setLastName("XXX");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("First Class");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("1234");
        reservationDetails.setContactEmail("XX@gmail.com");

        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization",myToken);
        HttpEntity<ReservationDetails> request = new HttpEntity<>(reservationDetails, headers);

        ResponseEntity<String> response = restTemplate.postForEntity("/reserve", request, String.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.BAD_REQUEST);
        assertThat(response.getBody()).contains("Contact number must be a 10-digit number");

    }

    /* Positive scenario 1:
   create the reservationDetails object  with fields firstName, lastName, gender, age,
   flightId, travelClass, ticketPrice, currencyCode,paymentMode, contactNumber
   and contactEmail attributes
   create the HTTP headers object and pass the jwtToken
   call /reserve end point using post method, pass reservationDetails as request body
   assert that http status OK
   and  assert that the response message contains "reservation made successfully"
   */
    @Test
    public void testPositiveScenario() {
        ReservationDetails reservationDetails = new ReservationDetails();
        reservationDetails.setFirstName("XXXX");
        reservationDetails.setLastName("XXX");
        reservationDetails.setGender("Male");
        reservationDetails.setAge(30);
        reservationDetails.setFlightId(1);
        reservationDetails.setTravelClass("First Class");
        reservationDetails.setTicketPrice(100.0);
        reservationDetails.setCurrencyCode("USD");
        reservationDetails.setPaymentMode("Credit Card");
        reservationDetails.setContactNumber("0928895717");
        reservationDetails.setContactEmail("phamhuutuanaws@gmail.com");

        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization",myToken);
        HttpEntity<ReservationDetails> request = new HttpEntity<>(reservationDetails, headers);
        ResponseEntity<String> response = restTemplate.postForEntity("/reserve", request, String.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
    }
}
```