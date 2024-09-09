---
title : "Build JPA Repository Interface"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

- Build JPA Repository Interfaces to acess the data from MYSQL Tables. Specify the query patterns required to get data from airport and flight table.
- Open the AirportRepository.java class under the src/main/java/com.airlines.catalog/repository folder and add the following imports.

![image.png](/images/module_1/jpa_repo/image.png)

- Use the following prompt to create the AirportRepository class.

{{< callout type="info" title="Prompt" >}}
*Add a method to find airport by airport code*
{{< /callout >}}

![image.png](/images/module_1/jpa_repo/image_1.png)

- Open the FlightRepository.java class under the src/main/java/com.airlines.catalog/repository folder and add the following imports.

![image.png](/images/module_1/jpa_repo/image_2.png)

- Use the following prompt to create the FlightRepository class.

{{< callout type="info" title="Prompt" >}}
*Create JPA repository interface FlightRepository.*<br>

*Add a method to find flights by departure date, departure airport code, arrival airport code that returns a iterable flight*<br>

*Add a method to get flight with Id as parameter and return the flight object*
{{< /callout >}}

![image.png](/images/module_1/jpa_repo/image_3.png)

- Complete code for **AirportRepository** class

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

- Complete code for **FlightRepository** class

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