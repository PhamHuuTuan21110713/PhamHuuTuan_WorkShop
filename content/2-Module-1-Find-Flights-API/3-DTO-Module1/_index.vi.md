---
title : "Tạo Data Transfer Object (DTO)"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

- Thực thể **Flight** mà bạn đã tạo trong chương " Xây dựng các lớp model " chứa mã sân bay nhưng phản hồi API cần thêm thông tin về sân bay như tên, thành phố và ngôn ngữ. Bạn sẽ tạo một lớp **FlightDetails** sẽ kết hợp thông tin từ các bảng **flight** và **airport**.
- Mở **FlightDetails.java** trong folder **src/main/java/com.airlines.catalog/dto**
- Từ **Amazon Q Chat Panel** trong IDE của bạn, hãy sử dụng prompt  bên dưới để tạo lớp **FlightDetails** hoàn chỉnh. Nhấp vào nút **Copy** bên dưới code panel và dán code vào tệp **FlightDetails.java.**

{{< callout type="info" title="Prompt" >}}
*Create class FlightDetails with following attributes:*<br>

*flightId as int, departureDate, departureTime, departureAirportCode as string, departureAirportName as string,*<br>

*departureAirportCity, departureAirportLocale,*<br>

*arrivalAirportCode, arrivalAirportName, arrivalAirportCity, arrivalAirportLocale,*<br>

*arrivalDate, arrivalTime, ticketPrice as double,*<br>

*ticketCurrency, flightNumber, flightDuration, seatAvailable as int*<br>
{{< /callout >}}


![image.png](/images/module_1/dto/image.png)

- Tạo một phương thức **toString** cho lớp này bằng cách sử dụng prompt bên dưới, sau đó copy code bỏ vào file **FlightDetails.java**

{{< callout type="info" title="Prompt" >}}
    *Create a tostring method to convert attributes to string*   
{{< /callout >}}


![image.png](/images/module_1/dto/image_1.png)

- Tương tự tạo 1 phương thức là **toJon** với prompt dưới đây.

{{< callout type="info" title="Prompt" >}}
*Create a toJson method to convert the attributes to Json string*
{{< /callout >}}

![image.png](/images/module_1/dto/image_2.png)

- Code hoàn chỉnh cho lớp **FlightDetails**

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