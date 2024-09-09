---
title : "Build the model classes"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

- We will build model classes to work with the **airport** table and the **flight** table. In the project we will see 4 files located in the directory **src/main/java/com.airlines.catalog/model.** For this module, we will work with **Airport.java** and **Flight.java**. We will build model classes in these two files.

![image.png](/images/module_1/model_classes/image.png)

- Open **Airport.java**. You want to know which libraries to import to automatically generate getters, setters and constructors. You also need to import the library as a **JPA Entiry**. Use the following prompt to get suggestions from the **Amazon Q Chat Panel**.

{{< callout type="info" title="Prompt1" >}}
*Give me the import statements that i need to include in my model class to generate Getter, Setter and constructor*
{{< /callout >}}


{{< callout type="info" title="Prompt2" >}}
*Give me the import statements that i need to include in my model class to make it a JPA Entity*
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_1.png)

![image.png](/images/module_1/model_classes/image_2.png)

- We will copy and paste these libraries into the **Airport.java.** file.
- Next, we use the prompt below to complete the **Airport.java** class.

{{< callout type="info" title="Prompt" >}}
*Create an Entity class Airport mapped to table airport with following 4 attributes; airportCode as id, airportName, airportCity and airportLocale.*<br>

*Each attribute should be mapped to table columns and column names will be same as attribute names with _ separator between parts of the name. Use Lombok for getters and setters*
{{< /callout >}}


![image.png](/images/module_1/model_classes/image_3.png)

- Open **DBSetupModule1.sql** and select the SQL command " **CREATE TABLE flight**", right click and select **Send to Amazon Q** and then **Send to Prompt** from the sub menu. The SQL command is copied to the **Amazon Q Chat Panel** and you can use the prompt below at the beginning of the SQL command to get the code for the Flight class.
  
{{< callout type="info" title="Prompt" >}}
*Create flight JPA Entity class for the below table defintiion. Do not create any relationship between airport and flight entity. Use String for date and time columns*   
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_4.png)

- After creating, copy the code and put it in the file **Flight.java**
- Create a **toString** method for this class using the prompt below, then copy the code into the **Flight.java** file

{{< callout type="info" title="Prompt" >}}
*Create a tostring method to convert attributes to string*
{{< /callout >}}

- Similarly, create a **toJson** method for this class using the prompt below, then, take the code and put it in the **Flight.java** file.

{{< callout type="info" title="Prompt" >}}
Create a **toJson** method to convert the attributes to Json String
{{< /callout >}}

![image.png](/images/module_1/model_classes/image_5.png)

![image.png](/images/module_1/model_classes/image_6.png)

- Complete code for **Airport** class

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

- Complete code for **Flight** class

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
