---
title : "Unit Testing cho các lớp Repository"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

- Bây giờ chúng ta sẽ unit test các lớp reposity. Việc kiểm tra sẽ được thực hiện bằng cách tích hợp trực tiếp với RDS (No Mocking).
- Mở **AirportRepositoryTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau

![image.png](/images/module_1/unit_test_repository/image.png)

- Sử dụng prompt bên dưới để xây dựng các unit test case cho lớp **AirportRepository**

{{< callout type="info" title="Prompt" >}}

*Create AirportRepositoryTest class to test the AirportRepository class using web environment with random port.*<br>

*First negative test case: method with Invalid airport code - "LHG" which should assert a null object*<br>

*Second positive test case: method with valid airport code - "LHR" which should assert a not null object*<br>

{{< /callout >}}

![image.png](/images/module_1/unit_test_repository/image_1.png)

- Bấm vào mũi tên bên trái khai báo lớp AirportRepositoryTest và chọn Run AirportRepositoryTest. Nó sẽ chạy tất cả các test case trong lớp. Nếu bất kỳ test nào không thành công, hãy kiểm tra lỗi springboot trong IDE. Nếu exception là do lỗi cấu hình HibernateJpaConfiguration, hãy kiểm tra tệp "**application.properties**" để đảm bảo các giá trị phù hợp được điền cho aws.zone và secretmanager.key. Đồng thời kiểm tra xem security group cho RDS có cho phép liên lạc từ IP của máy nơi chương trình này đang chạy hay không. Nếu Spring Boot khởi động thành công thì hãy kiểm tra mã xác nhận và xác minh rằng dữ liệu thử nghiệm trong bảng Airport đã được điền theo test case.
- Tôi đã tiến hành chạy và đều pass tất cả các test case.

![image.png](/images/module_1/unit_test_repository/image_2.png)

- Mở **FlightRepositoryTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau:

![image.png](/images/module_1/unit_test_repository/image_3.png)

- Xây dựng các Unit test case cho lớp **FlightRepository** bằng cách sử dụng prompt bên dưới.

{{< callout type="info" title="Prompt" >}}

*Create FlightRepositoryTest class to test the FlightRepository class using web environment with random port.*<br>

*Test cases for testing findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode method that returns an iterable.*<br>

*First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG" and arrival airport code as "LAX".*<br>

*Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR" and arrival airport code as "CDG" .*<br>

*Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "MIA" and arrival airport code as "LAX".*<br>

{{< /callout >}}

![image.png](/images/module_1/unit_test_repository/image_4.png)

- Nhấp vào mũi tên ở bên trái khai báo lớp **FlightRepositoryTest** và chọn Run **FlightRepositoryTest**. Nó sẽ chạy tất cả các test case trong lớp. Nếu bất kỳ test nào không thành công, hãy kiểm tra lỗi springboot trong IDE. Nếu exception là do lỗi cấu hình HibernateJpaConfiguration, hãy kiểm tra tệp "**application.properties**" để đảm bảo các giá trị phù hợp được điền cho aws.zone và secretmanager.key. Nếu Spring Boot khởi động thành công thì hãy kiểm tra mã xác nhận và xác minh rằng dữ liệu thử nghiệm trong flight bay đã được điền theo trường hợp thử nghiệm.
- Tôi đã chạy và cả 3 test case đều pass

![image.png](/images/module_1/unit_test_repository/image_5.png)

- Code hoàn chỉnh cho lớp **FlightRepositoryTest**

```java
package com.airlines.catalog.test;
import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.model.Flight;
import com.airlines.catalog.repository.FlightRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

/*
Create FlightRepositoryTest class to test the FlightRepository class using web environment with random port.
Test cases for testing findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode method that returns an iterable.
First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG"
and arrival airport code as "LAX".
Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR" and arrival airport code as "CDG" .
Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "MIA" and arrival airport code as "LAX".
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightRepositoryTest {

    @Autowired
    private FlightRepository flightRepository;

    @Value("${local.server.port}")
    private int port;

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_NoResults() {
        String departureDate = "2023-08-01";
        String departureAirportCode = "CDG";
        String arrivalAirportCode = "LAX";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertFalse(flights.iterator().hasNext());
    }

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_SingleResult() {
        String departureDate = "2023-08-02";
        String departureAirportCode = "LHR";
        String arrivalAirportCode = "CDG";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertTrue(flights.iterator().hasNext());
        Assert.assertEquals(1, flights.spliterator().getExactSizeIfKnown());
    }

    @Test
    public void findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode_MultipleResults() {
        String departureDate = "2023-08-01";
        String departureAirportCode = "MIA";
        String arrivalAirportCode = "LAX";
        Iterable<Flight> flights = flightRepository.findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(departureDate, departureAirportCode, arrivalAirportCode);
        Assert.assertNotNull(flights);
        Assert.assertTrue(flights.iterator().hasNext());
        Assert.assertEquals(2, flights.spliterator().getExactSizeIfKnown());
    }
}
```