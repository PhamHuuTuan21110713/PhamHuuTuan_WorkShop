---
title : "Create Data Transfer Object (DTO)"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

- Flight entity you created as part of "Build the model classes" chapter contains the airport code but API response needs additional information about the airport such as name, city and locale. You will create a FlightDetails class that will combine the information from flight and airport tables.
- Open FlightDetails.java under the src/main/java/com.airlines.catalog/dto folder.
- From the Amazon Q Chat Panel in your IDE, use the below prompt to create the complete FlightDetails class. Click on Copy button below the code panel and paste the code into the FlightDetails.java file.

{{< callout type="info" title="Prompt" >}}
*Create class FlightDetails with following attributes:*<br>

*flightId as int, departureDate, departureTime, departureAirportCode as string, departureAirportName as string,*<br>

*departureAirportCity, departureAirportLocale,*<br>

*arrivalAirportCode, arrivalAirportName, arrivalAirportCity, arrivalAirportLocale,*<br>

*arrivalDate, arrivalTime, ticketPrice as double,*<br>

*ticketCurrency, flightNumber, flightDuration, seatAvailable as int*<br>
{{< /callout >}}


![image.png](/images/module_1/dto/image.png)

- Create a **toString** method for this class using the prompt below, then copy the code and put it into the **FlightDetails.java** file

{{< callout type="info" title="Prompt" >}}
    *Create a tostring method to convert attributes to string*   
{{< /callout >}}


![image.png](/images/module_1/dto/image_1.png)

- Similarly create a method **toJon** with the prompt below.

{{< callout type="info" title="Prompt" >}}
*Create a toJson method to convert the attributes to Json string*
{{< /callout >}}

![image.png](/images/module_1/dto/image_2.png)

- Complete code for **FlightDetails** class

```java
package com.airlines.catalog.dto;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class FlightDetails {
    private int flightId;
    private String departureDate;
    private String departureTime;
    private String departureAirportCode;
    private String departureAirportName;
    private String departureAirportCity;
    private String departureAirportLocale;
    private String arrivalAirportCode;
    private String arrivalAirportName;
    private String arrivalAirportCity;
    private String arrivalAirportLocale;
    private String arrivalDate;
    private String arrivalTime;
    private double ticketPrice;
    private String ticketCurrency;
    private String flightNumber;
    private int flightDuration;
    private int seatAvailable;

//Create a toString method to convert the attributes to string
@Override
public String toString() {
    return "FlightDetails{" +
            "flightId=" + flightId +
            ", departureDate='" + departureDate + '\'' +
            ", departureTime='" + departureTime + '\'' +
            ", departureAirportCode='" + departureAirportCode + '\'' +
            ", departureAirportName='" + departureAirportName + '\'' +
            ", departureAirportCity='" + departureAirportCity + '\'' +
            ", departureAirportLocale='" + departureAirportLocale + '\'' +
            ", arrivalAirportCode='" + arrivalAirportCode + '\'' +
            ", arrivalAirportName='" + arrivalAirportName + '\'' +
            ", arrivalAirportCity='" + arrivalAirportCity + '\'' +
            ", arrivalAirportLocale='" + arrivalAirportLocale + '\'' +
            ", arrivalDate='" + arrivalDate + '\'' +
            ", arrivalTime='" + arrivalTime + '\'' +
            ", ticketPrice=" + ticketPrice +
            ", ticketCurrency='" + ticketCurrency + '\'' +
            ", flightNumber='" + flightNumber + '\'' +
            ", flightDuration=" + flightDuration +
            ", seatAvailable=" + seatAvailable +
            '}';
}
    //Create a toJson method to convert the attributes to Json string
    public String toJson() {
        return "{" +
                "\"flightId\":\"" + flightId + '\"' +
                ", \"departureDate\":\"" + departureDate + '\"' +
                ", \"departureTime\":\"" + departureTime + '\"' +
                ", \"departureAirportCode\":\"" + departureAirportCode + '\"' +
                ", \"departureAirportName\":\"" + departureAirportName + '\"' +
                ", \"departureAirportCity\":\"" + departureAirportCity + '\"' +
                ", \"departureAirportLocale\":\"" + departureAirportLocale + '\"' +
                ", \"arrivalAirportCode\":\"" + arrivalAirportCode + '\"' +
                ", \"arrivalAirportName\":\"" + arrivalAirportName + '\"' +
                ", \"arrivalAirportCity\":\"" + arrivalAirportCity + '\"' +
                ", \"arrivalAirportLocale\":\"" + arrivalAirportLocale + '\"' +
                ", \"arrivalDate\":\"" + arrivalDate + '\"' +
                ", \"arrivalTime\":\"" + arrivalTime + '\"' +
                ", \"ticketPrice\":\"" + ticketPrice + '\"' +
                ", \"ticketCurrency\":\"" + ticketCurrency + '\"' +
                ", \"flightNumber\":\"" + flightNumber + '\"' +
                ", \"flightDuration\":\"" + flightDuration + '\"' +
                ", \"seatAvailable\":\"" + seatAvailable + '\"' +
                '}';
}
}
```