---
title : "Dựng JPA Repository Interface"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

- Xây dựng JPA Repository Interface để truy cập dữ liệu từ Bảng MYSQL. Chỉ định các mẫu truy vấn cần thiết để lấy dữ liệu từ bảng **airport** và bảng **flight**.
- Mở lớp **AirportRepository.java** trong thư mục **src/main/java/com.airlines.catalog/repository** và thêm các import sau.

![image.png](/images/module_1/jpa_repo/image.png)

- Sử dụng prompt sau để tạo lớp **AirportRepository**.

{{< callout type="info" title="Prompt" >}}
*Add a method to find airport by airport code*
{{< /callout >}}

![image.png](/images/module_1/jpa_repo/image_1.png)

- Mở lớp **FlightRepository.java** trong thư mục.

![image.png](/images/module_1/jpa_repo/image_2.png)

- Sử dụng prompt sau để tạo lớp **FlightRepository**.

{{< callout type="info" title="Prompt" >}}
*Create JPA repository interface FlightRepository.*<br>

*Add a method to find flights by departure date, departure airport code, arrival airport code that returns a iterable flight*<br>

*Add a method to get flight with Id as parameter and return the flight object*
{{< /callout >}}

![image.png](/images/module_1/jpa_repo/image_3.png)

- Code hoàn chỉnh cho lớp **AirportRepository**

```java
package com.airlines.catalog.repository;

import com.airlines.catalog.model.Airport;
import org.springframework.stereotype.Repository;
import org.springframework.data.jpa.repository.JpaRepository;

/* Create JPA repository interface named AirportRepository
Add a method to find airport by airport code */
@Repository
public interface AirportRepository extends JpaRepository<Airport, String> {
    Airport findByAirportCode(String airportCode);
}
```

- Code hoàn chỉnh cho lớp **FlightRepository**

```java
package com.airlines.catalog.repository;

import com.airlines.catalog.model.Flight;
import org.springframework.stereotype.Repository;
import org.springframework.data.jpa.repository.JpaRepository;

/*Create JPA repository interface FlightRepository.
Add a method to find flights by departure date, departure airport code, arrival airport code that returns a iterable flight
Add a method to get flight with Id as parameter and return the flight object
*/
@Repository
public interface FlightRepository extends JpaRepository<Flight, Integer> {
    Iterable<Flight> findByDepartureDateAndDepartureAirportCodeAndArrivalAirportCode(String departureDate, String departureAirportCode, String arrivalAirportCode);
    Flight findById(int id);
}
```