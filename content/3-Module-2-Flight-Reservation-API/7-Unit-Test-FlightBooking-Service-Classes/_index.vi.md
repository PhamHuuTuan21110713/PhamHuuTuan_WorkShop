---
title : "Unit Test Lớp service FlightBooking"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 3.7 </b> "
---


- Bây giờ chúng ta sẽ xây dựng unit test script cho FlightBooking service với Junit. Các test script sẽ tích hợp với các dịch vụ phụ trợ và dịch vụ AWS.
- Mở **FlightBookingServiceTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các câu import sau.

![image.png](/images/module_2/unit_test_service_flightbooking/image.png)

- Tạo lớp để test **FlightBooking** service, dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

Create FlightBookingServiceTest class to test the flightBooking service using web environment with random port.
Autowire sns.arn, aws.region from properties file

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_1.png)
- Kịch bản thử nghiệm đầu tiên - việc đặt trước thành công với các tham số đầu vào hợp lệ. Việc đặt chỗ phải thành công với dữ liệu được lưu trong hành khác (passenger), bảng đặt chỗ (reservation) và nhận được email thông báo.

{{< callout type="info" title="Prompt" >}}

Create reserve flight success test method.
Create Passenger object by setting attributes firstName, lastName, Adult=true and gender=male
create Reservation object class by setting attributes flightId=1,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus,passengerId, reservationDate,
reservationTime, paymentStatus
Create AWS region object

{{< /callout >}}

- Thêm đoạn code này để gọi phương thức **reserveFlight**

{{< callout type="info" title="Code" >}}

Assert.assertTrue(flightBooking.reserveFlight(passenger, reservation, passengerRepository, reservationRepository, flightRepository, 1, snsArn, region));

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_2.png)
- Kịch bản thử nghiệm thứ hai - kịch bản thất bại trong việc đặt chỗ với id chuyến bay không hợp lệ làm đầu vào

{{< callout type="info" title="Prompt" >}}

Create  reserve flight invalid flightId test method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=10000,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId, reservationDate,
reservationTime, paymentStatus
Create AWS region object
asset if the exception contains 'No flights found"


{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_3.png)
- Nếu khối lệnh try/catch không được thêm vào xung quanh phương thức **ReserveFlight**, vui lòng thêm vào. Amazon Q developer sẽ cung cấp đề xuất theo từng dòng

![image.png](/images/module_2/unit_test_service_flightbooking/image_4.png)
- Cập nhật số cột còn trống (**seat_available**) trong bảng chuyến bay (**flight**) thành 0 cho id=5 bằng cách chạy câu lệnh SQL bên dưới trong MYSQL Workbench.

{{< callout type="info" title="Code" >}}

 update flight set seat_available =0 where id=5; 

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_5.png)
- Kịch bản thử nghiệm thứ ba - kịch bản đặt chỗ thất bại vì không còn chỗ trống cho id chuyến bay được yêu cầu.

{{< callout type="info" title="Prompt" >}}


Create  reserve flight insufficient seats method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=5,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId,
reservationDate, reservationTime, paymentStatus
Create AWS region object.
asset if the exception contains 'Seats not available".
 

{{< /callout >}}

![image.png](/images/module_2/unit_test_service_flightbooking/image_6.png)
- Một lần nữa, nếu khối code **try/catch** không được thêm vào xung quanh phương thức **ReserveFlight**, vui lòng thêm vào. Amazon Q developer  sẽ cung cấp đề xuất theo từng dòng

![image.png](/images/module_2/unit_test_service_flightbooking/image_7.png)
- Chúng ta sẽ tiến hành chạy các test case vừa tạo, và tôi đã pass 3 test case

![image.png](/images/module_2/unit_test_service_flightbooking/image_8.png)
- Tiếp theo ta check database để xem các test case đặt chỗ thành công có được thêm data vào database hay không. Chúng ta sẽ check dữ liệu ở cả bảng passenger và reservation, tiếp đó là chúng ta sẽ nhận được email thông báo, đáp ứng các điều trên thì tôi đã thành công.

![image.png](/images/module_2/unit_test_service_flightbooking/image_9.png)
![image.png](/images/module_2/unit_test_service_flightbooking/image_10.png)
![image.png](/images/module_2/unit_test_service_flightbooking/image_11.png)
- Code hoàn chỉnh cho lớp **FlightBookingServiceTest**

```java
package com.airlines.catalog.test;

import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.model.Reservation;
import com.airlines.catalog.model.Passenger;
import com.airlines.catalog.repository.ReservationRepository;
import com.airlines.catalog.repository.PassengerRepository;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.service.FlightBooking;
import com.airlines.catalog.exception.FlightNotFoundException;
import com.airlines.catalog.exception.RequestedSeatsNotAvailable;
import software.amazon.awssdk.regions.Region;

/*
Create FlightBookingTest class to test the FlightBooking service using
web environment with random port.
Autowire sns.arn, aws.region from properties file
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightBookingServiceTest {

    @Autowired
    FlightBooking flightBooking;

    @Autowired
    FlightRepository flightRepository;

    @Autowired
    PassengerRepository passengerRepository;

    @Autowired
    ReservationRepository reservationRepository;

    @Value("${sns.arn}")
    String snsArn;

    @Value("${aws.region}")
    String awsRegion;

    /*
    Create reserve flight success test method.
    Create Passenger object by setting attributes firstName, lastName, Adult=true and gender=male
    create Reservation object class by setting attributes flightId=1,travelClass, ticketPrice,
    currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus,passengerId, reservationDate,
    reservationTime, paymentStatus
    Create AWS region object
    */
    @Test
    public void reserveFlightSuccessTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("XXXX");
        passenger.setLastName("XXX");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(1);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("abc@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);

        Assert.assertTrue(flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                reservationRepository, flightRepository, 1, snsArn, region));

    }

    /*
    Create  reserve flight invalid flightId test method.
    Create passenger object by setting attributes firstName, lastName, Adult=true and gender
    Create reservation object class by setting attributes flightId=10000,travelClass, ticketPrice,
    currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId, reservationDate,
    reservationTime, paymentStatus
    Create AWS region object
    asset if the exception contains 'No flights found"
    */
    @Test
    public void reserveFlightInvalidFlightIdTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("XXXX");
        passenger.setLastName("XXX");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(10000);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("abc@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);
        try {
            Assert.assertThrows(FlightNotFoundException.class, () -> flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                    reservationRepository, flightRepository, 10000, snsArn, region));
        } catch (FlightNotFoundException e) {
            Assert.assertEquals("No flights found", e.getMessage());
        }

    }

    /*
Create  reserve flight insufficient seats method.
Create passenger object by setting attributes firstName, lastName, Adult=true and gender
Create reservation object class by setting attributes flightId=5,travelClass, ticketPrice,
currencyCode, paymentMode,contactNumber, contactEmail, reservationStatus, passengerId,
reservationDate, reservationTime, paymentStatus
Create AWS region object.
asset if the exception contains 'Seats not available".
*/
    @Test
    public void reserveFlightInsufficientSeatsTest() {
        Passenger passenger = new Passenger();
        passenger.setFirstName("Test");
        passenger.setLastName("Test");
        passenger.setAdult(true);
        passenger.setGender("male");
        Reservation reservation = new Reservation();
        reservation.setFlightId(5);
        reservation.setTravelClass("economy");
        reservation.setTicketPrice(100);
        reservation.setCurrencyCode("USD");
        reservation.setPaymentMode("credit card");
        reservation.setContactNumber("1234567890");
        reservation.setContactEmail("test@gmail.com");
        reservation.setReservationStatus("pending");
        reservation.setPaymentStatus("Paid");
        reservation.setReservationDate("2023-10-25");
        reservation.setReservationTime("12:00:00");
        reservation.setPassengerId(passenger.getPassengerId());
        Region region = Region.of(awsRegion);
        try {
            Assert.assertThrows(RequestedSeatsNotAvailable.class, () -> flightBooking.reserveFlight(passenger, reservation, passengerRepository,
                    reservationRepository, flightRepository, 5, snsArn, region));
        } catch (RequestedSeatsNotAvailable e) {
            Assert.assertEquals("Seats not available", e.getMessage());
        }
    }
}
```