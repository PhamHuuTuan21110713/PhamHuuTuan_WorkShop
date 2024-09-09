---
title : "Unit Test lớp service"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---


- Bây giờ bạn sẽ unit test lớp **FlightDetailsService** bằng Junit. Các test case này sẽ xác thực luồng từ service đến cơ sở dữ liệu.
- Mở **FlightDetailsServiceTest.java** trong thư mục **test/java/com.airlines.catalog.test/** và thêm các import sau.

![image.png](/images/module_1/unit_test_service/image.png)

- Tạo lớp kiểm tra **FlightDetailsService**
- Kịch bản thử nghiệm đầu tiên: Không tìm thấy chuyến bay nào có ngày khởi hành, thành phố xuất phát và điểm đến. Dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG" and arrival airport code as "LAX", flightresultsRepository and airportresultsRepository as parameters. Assert count as 0

{{< /callout >}}

- Kịch bản thử nghiệm thứ hai: Chỉ tìm thấy một chuyến bay có ngày khởi hành, thành phố xuất phát và điểm đến, dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR", arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 1

{{< /callout >}}

- Kịch bản thử nghiệm thứ ba: Tìm thấy hai chuyến bay có ngày khởi hành, thành phố xuất phát và điểm đến, dùng prompt sau.

{{< callout type="info" title="Prompt" >}}

Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "LHR", arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 2

{{< /callout >}}

- Sau khi hoàn tất file, thì chúng ta nhấp vào mũi tên ở bên trái khai báo lớp **FlightDetailsServiceTest** và chọn **Run FlightDetailsServiceTest**. Nó sẽ chạy tất cả các test case trong lớp. Nếu bất kỳ test case nào không thành công, hãy kiểm tra lỗi springboot trong IDE.

![image.png](/images/module_1/unit_test_service/image_1.png)

- Code hoàn chỉnh cho lớp **FlightDetailsServiceTest**

```java
package com.airlines.catalog.test;

import com.airlines.catalog.FlightBookingApplication;
import com.airlines.catalog.dto.FlightDetails;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.repository.AirportRepository;
import com.airlines.catalog.service.FlightDetailsService;
import org.junit.Assert;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.ArrayList;
import java.util.List;

/*
Create FlightDetailsServiceTest class to test the FlightDetailsService using
web environment with random port. Test cases for testing findFlights method in this class which  returns
list of flightDetails.
*/
@SpringBootTest(classes = FlightBookingApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class FlightDetailsServiceTest {

    @Autowired
    FlightDetailsService flightDetailsService;

    @Autowired
    FlightRepository flightRepository;

    @Autowired
    AirportRepository airportRepository;

    @Value("${server.port}")
    private int port;

/*
First test case method with no results using departure date as "2023-08-01", departure airport code as "CDG"
and arrival airport code as "LAX", flightresultsRepository and airportresultsRepository as parameters. Assert count as 0
*/
    @Test
    public void findFlightsTest() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-01", "CDG", "LAX", flightRepository, airportRepository);
        Assert.assertEquals(0, flightDetailsList.size());
    }
    /*
Second test case method with single results using departure date as "2023-08-02", departure airport code as "LHR",
 arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 1
 */
    @Test
    public void findFlightsTest2() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-02", "LHR", "CDG", flightRepository, airportRepository);
        Assert.assertEquals(1, flightDetailsList.size());
    }
    /*
Third test case method with multiple results using departure date as "2023-08-01", departure airport code as "LHR",
arrival airport code as "CDG", flightresultsRepository and airportresultsRepository as parameters. Assert count as 2
*/
    @Test
    public void findFlightsTest3() {
        List<FlightDetails> flightDetailsList = flightDetailsService.findFlights("2023-08-01", "LHR", "CDG", flightRepository, airportRepository);
        Assert.assertEquals(2, flightDetailsList.size());
    }
}
```