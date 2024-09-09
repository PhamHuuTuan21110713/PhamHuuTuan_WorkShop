---
title : "Xây dựng JPA Repository"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

- Bạn sẽ tạo JPA Repository Interfaces để truy cập dữ liệu từ Bảng MYSQL. Chỉ định các mẫu truy vấn để lưu dữ liệu vào bảng passenger và reservation
- Mở **PassengerRepository.java** trong thư mục **src/main/java/com.airlines.catalog/repository**. Sau đó thêm các import này vào.

![image.png](/images/module_2/jpa_repository/image.png)

- Sử dụng prompt sau để tạo lớp **PassengerRepository.**

{{< callout type="info" title="Prompt" >}}

create jpa repository interface PassengerRepository.
Add a method to save passenger

{{< /callout >}}

![image.png](/images/module_2/jpa_repository/image_1.png)

- Mở **DepositRepository.java** trong thư mục **src/main/java/com.airlines.catalog/repository**, và thêm các import sau.

![image.png](/images/module_2/jpa_repository/image_2.png)

- Sử dụng prompt sau để tạo lớp DepositRepository.

{{< callout type="info" title="Prompt" >}}

Create interface ReservationRepository that extends JpaRepository.
Add a method to save reservation.

{{< /callout >}}

![image.png](/images/module_2/jpa_repository/image_3.png)

- Mã hoàn chỉnh cho lớp PassengerRepository
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

- Code hoàn chỉnh cho lớp **ReservationRepository**

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