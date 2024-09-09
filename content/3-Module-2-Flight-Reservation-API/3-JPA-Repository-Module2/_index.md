---
title : "Build the JPA Repository"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

- You will create JPA Repository Interfaces to acess the data from MYSQL Tables. Specify the query patterns to save the data to passenger and reservation table
- Open the PassengerRepository.java under the src/main/java/com.airlines.catalog/repository folder, then add the following imports in the document.

![image.png](/images/module_2/jpa_repository/image.png)

- Use the following prompt to create the PassengerRepository class.

{{< callout type="info" title="Prompt" >}}

create jpa repository interface PassengerRepository.
Add a method to save passenger

{{< /callout >}}

![image.png](/images/module_2/jpa_repository/image_1.png)

- Open the ReservationRepository.java under the src/main/java/com.airlines.catalog/repository folder, then Add the following imports in the document.

![image.png](/images/module_2/jpa_repository/image_2.png)
- Use the following prompt to create the ReservationRepository class.

{{< callout type="info" title="Prompt" >}}

Create interface ReservationRepository that extends JpaRepository.
Add a method to save reservation.

{{< /callout >}}

![image.png](/images/module_2/jpa_repository/image_3.png)

- Complete code for PassengerRepository class
```java
package com.airlines.catalog.repository;

import com.airlines.catalog.model.Passenger;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;


/* create jpa repository interface PassengerRepository.
Add a method to save Passenger. */
@Repository
public interface PassengerRepository extends JpaRepository<Passenger, Integer> {
    Passenger save(Passenger passenger);
}

```


- Complete code for ReservationRepository class

```java
package com.airlines.catalog.repository;

import com.airlines.catalog.model.Reservation;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

// Create interface ReservationRepository that extends JpaRepository.
// Add a method to save reservation.
@Repository
public interface ReservationRepository extends JpaRepository<Reservation, Long> {

    Reservation save(Reservation reservation);
}
```