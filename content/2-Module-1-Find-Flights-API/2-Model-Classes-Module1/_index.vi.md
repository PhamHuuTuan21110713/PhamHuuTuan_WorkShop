---
title : "Xây dựng các lớp model"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

- Chúng ta sẽ xây dựng các lớp model để làm việc với bảng **airport** và bảng **flight**. Trong project chúng ta sẽ thấy có 4 file nằm trong directory **src/main/java/com.airlines.catalog/model.** Đối với module này, chúng ta sẽ làm việc với **Airport.java** và **Flight.java**. Chúng ta sẽ xây dựng các lớp modle trong hai tệp này.

![image.png](/images/module_1/model_classes/image.png)

- Mở **Airport.java**. Bạn muốn biết các thư viện cần nạp để tạo tự động các getters, setters và constructor. Bạn cũng cần nhập thư viện làm **JPA Entiry**. Sử dụng prompt sau để nhận đề xuất từ  **Amazon Q Chat Panel**.

{{< callout type="info" title="Prompt1" >}}
*Give me the import statements that i need to include in my model class to generate Getter, Setter and constructor*
{{< /callout >}}


{{< callout type="info" title="Prompt2" >}}
*Give me the import statements that i need to include in my model class to make it a JPA Entity*
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_1.png)

![image.png](/images/module_1/model_classes/image_2.png)

- Chúng ta sẽ copy và dán các thư viện này vào file **Airport.java.**
- Tiếp theo, chúng ta dùng prompt dưới đây để tiến hành hoàn thành lớp **Airport.java**

{{< callout type="info" title="Prompt" >}}
*Create an Entity class Airport mapped to table airport with following 4 attributes; airportCode as id, airportName, airportCity and airportLocale.*<br>

*Each attribute should be mapped to table columns and column names will be same as attribute names with _ separator between parts of the name. Use Lombok for getters and setters*
{{< /callout >}}


![image.png](/images/module_1/model_classes/image_3.png)

- Mở **DBSetupModule1.sql** và chọn lệnh SQL " **CREATE TABLE flight**", nhấp chuột phải và chọn **Send to Amazon Q** rồi sau đó **Send to Prompt** từ menu phụ. Lệnh SQL được sao chép vào **Amazon Q Chat Panel** và bạn có thể sử dụng prompt bên dưới ở đầu câu lệnh SQL để lấy code cho lớp Flight.
  
{{< callout type="info" title="Prompt" >}}
*Create flight JPA Entity class for the below table defintiion. Do not create any relationship between airport and flight entity. Use String for date and time columns*   
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_4.png)

- Sau khi tạo xong thì copy code bỏ vào file **Flight.java**
- Tạo một phương thức **toString** cho lớp này bằng cách sử dụng prompt bên dưới, sau đó copy code bỏ vào file **Flight.java**

{{< callout type="info" title="Prompt" >}}
*Create a tostring method to convert attributes to string*
{{< /callout >}}

- Tượng tự, tạo một phương thức **toJson** cho lớp này bằng cách sử dụng prompt bên dưới, sau đó, lấy code bỏ vào file **Flight.java**

{{< callout type="info" title="Prompt" >}}
Create a **toJson** method to convert the attributes to Json String
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_5.png)

![image.png](/images/module_1/model_classes/image_6.png)

- Code hoàn chỉnh cho lớp **Airport**

```java
    package com.airlines.catalog.model;
    package com.airlines.catalog.model;
    import javax.persistence.Entity;
    import javax.persistence.Id;
    import javax.persistence.Table;
    import javax.persistence.Column;
    import lombok.Data;

    @Entity
    @Table(name = "airport")
    @Data
    public class Airport {

        @Id
        @Column(name = "airport_code")
        private String airportCode;

        @Column(name = "airport_name")
        private String airportName;

        @Column(name = "airport_city")
        private String airportCity;

        @Column(name = "airport_locale")
        private String airportLocale;

        // Default constructor (required by JPA)
        public Airport() {
        }
}
```

- Code hoàn chỉnh cho lớp **Flight**

```java
package com.airlines.catalog.model;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Column;
import lombok.Getter;
import lombok.Setter;

@Entity
@Table(name = "flight")
@Getter
@Setter
public class Flight {

    @Id
    @Column(name = "id")
    private int id;

    @Column(name = "departure_date")
    private String departureDate;

    @Column(name = "departure_time")
    private String departureTime;

    @Column(name = "departure_airport_code")
    private String departureAirportCode;

    @Column(name = "arrival_date")
    private String arrivalDate;

    @Column(name = "arrival_time")
    private String arrivalTime;

    @Column(name = "arrival_airport_code")
    private String arrivalAirportCode;

    @Column(name = "flight_number")
    private String flightNumber;

    @Column(name = "flight_duration")
    private int flightDuration;

    @Column(name = "ticket_price")
    private double ticketPrice;

    @Column(name = "ticket_currency")
    private String ticketCurrency;

    @Column(name = "seat_capacity")
    private int seatCapacity;

    @Column(name = "seat_available")
    private int seatAvailable;

    // Default constructor
    public Flight() {
    }

    //Create a tostring method to convert attributes to string
    @Override
    public String toString() {
        return "Flight{" +
                "id=" + id +
                ", departureDate='" + departureDate + '\'' +
                ", departureTime='" + departureTime + '\'' +
                ", departureAirportCode='" + departureAirportCode + '\'' +
                ", arrivalDate='" + arrivalDate + '\'' +
                ", arrivalTime='" + arrivalTime + '\'' +
                ", arrivalAirportCode='" + arrivalAirportCode + '\'' +
                ", flightNumber='" + flightNumber + '\'' +
                ", flightDuration=" + flightDuration +
                ", ticketPrice=" + ticketPrice +
                ", ticketCurrency='" + ticketCurrency + '\'' +
                ", seatCapacity=" + seatCapacity +
                ", seatAvailable=" + seatAvailable +
                '}';
    }

    //Create a toJson method to convert the attributes to Json String
    public String toJson() {
        return "{" +
                "\"id\":\"" + id + '\"' +
                ", \"departureDate\":\"" + departureDate + '\"' +
                ", \"departureTime\":\"" + departureTime + '\"' +
                ", \"departureAirportCode\":\"" + departureAirportCode + '\"' +
                ", \"arrivalDate\":\"" + arrivalDate + '\"' +
                ", \"arrivalTime\":\"" + arrivalTime + '\"' +
                ", \"arrivalAirportCode\":\"" + arrivalAirportCode + '\"' +
                ", \"flightNumber\":\"" + flightNumber + '\"' +
                ", \"flightDuration\":" + flightDuration +
                ", \"ticketPrice\":" + ticketPrice +
                ", \"ticketCurrency\":\"" + ticketCurrency + '\"' +
                ", \"seatCapacity\":" + seatCapacity +
                ", \"seatAvailable\":" + seatAvailable +
                '}';
    }
}
```
