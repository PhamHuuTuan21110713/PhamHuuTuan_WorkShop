---
title : "Xây dựng các lớp service"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 3.6 </b> "
---

- Bây giờ chúng ta sẽ xây dựng lớp service **FlightBooking** tạo database transaction mới để lưu hành khách, dữ liệu đặt chỗ vào các bảng tương ứng và cũng gửi thông báo SNS đặt chỗ-thành công ( reservation-success topic). Một thông báo email sẽ được gửi đến địa chỉ email được cấu hình. Phương pháp này cũng sẽ kiểm tra tình trạng chỗ trống trước khi tiến hành đặt chỗ và đưa ra ngoại lệ kinh doanh nếu không có chỗ.
- Mở **FlightBooking.java** trong thư mục **src/main/java/com.airlines.catalog/service** và thêm các import sau.

![image.png](/images/module_2/service_classes/image.png)

- Trong lớp **FlightBooking**, hãy sử dụng prompt sau để tạo phương thức **sendNotification**. Điều này sẽ gửi thông báo đến **SNS topic**.

{{< callout type="info" title="Prompt" >}}

Create a method send message with message, sns arn and AWS Region as input parameter 

{{< /callout >}}

![image.png](/images/module_2/service_classes/image_1.png)

- Thêm một phương thức khác **ReserveFlight** bằng cách sử dụng prompt bên dưới. Điều này sẽ thực hiện logic cần thiết để xác thực dữ liệu, lưu dữ liệu bằng các phương thức repository được tạo trước đó và gửi thông báo bằng phương thức **sendNotification** đã tạo ở trên.

{{< callout type="info" title="Prompt" >}}

Create reserveFlight method with passenger, reservation, passengerRepository, reservationRepository,flightresultsRepository, number of passengers, topicArn and region as input parameters. Method should return a Boolean output
Get the flight details by calling findById method from FlightresultsRepository with flightId as input parameter if no of flight is null throw FlightNotFoundException
Call getSeatAvailable from flight object to get the available seats
If number of seats is less than number of passengers throw RequestedSeatsNotAvailable exception
If seats available is greater than or equal to number of passengers then
decrease the seats available attribute in flight object by calling setseatAvailable method
Save the passenger and get the passenger id
update the reservation object with the passenger id save the reservation details call sendMessage by passing reservation json string as input parameter
Method should return True or False based on the result of save methods
All of the above steps should be done within a transaction.

{{< /callout >}}

![image.png](/images/module_2/service_classes/image_2.png)

- Code hoàn chỉnh cho phương thức **sendNotification**

```java
@Service
public class FlightBooking {
    /* create a method send message with message, sns arn and AWS Region as input parameter */
    public static void sendMessage(String message, String topicArn, Region region) {
        SnsClient snsClient = SnsClient.builder()
                .region(region)
                .build();

        PublishRequest request = PublishRequest.builder()
                .message(message)
                .topicArn(topicArn)
                .build();

        PublishResponse result = snsClient.publish(request);
        System.out.println("Message sent successfully. Message ID: " + result.messageId());
    }

}
```

- Code hoàn chỉnh cho lớp **FlightBooking**

```java
package com.airlines.catalog.service;

import com.airlines.catalog.exception.FlightNotFoundException;
import com.airlines.catalog.exception.RequestedSeatsNotAvailable;
import com.airlines.catalog.model.Flight;
import com.airlines.catalog.model.Passenger;
import com.airlines.catalog.model.Reservation;
import com.airlines.catalog.repository.FlightRepository;
import com.airlines.catalog.repository.PassengerRepository;
import com.airlines.catalog.repository.ReservationRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.sns.SnsClient;
import software.amazon.awssdk.services.sns.model.PublishRequest;
import software.amazon.awssdk.services.sns.model.PublishResponse;

@Service
public class FlightBooking {

    /* create a method send message with sns arn and AWS Region as input parameter*/
    public void sendMessage(String message, String arn, Region region) {
        SnsClient snsClient = SnsClient.builder()
                .region(region)
                .build();

        PublishRequest request = PublishRequest.builder()
                .message(message)
                .topicArn(arn)
                .build();

        PublishResponse result = snsClient.publish(request);
        System.out.println(result.messageId() + " Message sent. Status is " + result.sdkHttpResponse().statusCode());
        snsClient.close();

    }

    /* Create reserveFlight method with passenger, reservation, passengerRepository,
reservationRepository,flightresultsRepository, number of passengers, topicArn and region
as input parameters. Method should return a Boolean output
Get the flight details by calling findById method from FlightresultsRepository with
flightId as input parameter
if no of flight is null throw FlightNotFoundException
Call getSeatAvailable from flight object to get the available seats
If number of seats is less than number of passengers throw RequestedSeatsNotAvailable exception
If seats available is greater than or equal to number of passengers then
    decrease the seats available attribute in flight object by calling setseatAvailable method
    Save the passenger and get the passenger id
    update the reservation object with the passenger id
    save the reservation details
    call sendMessage by passing reservation json string as input parameter
    Method should return True or False based on the result of save methods
All of the above steps should be done within a transaction.
 */
    @Transactional(propagation = Propagation.REQUIRED)
    public Boolean reserveFlight(Passenger passenger, Reservation reservation,
                                 PassengerRepository passengerRepository, ReservationRepository reservationRepository, FlightRepository flightRepository,
                                 int numberOfPassengers, String topicArn, Region region) throws FlightNotFoundException, RequestedSeatsNotAvailable {
        Flight flight = flightRepository.findById(reservation.getFlightId());
        if (flight == null) {
            throw new FlightNotFoundException(reservation.getFlightId());
        }
        if (flight.getSeatAvailable() < numberOfPassengers) {
            throw new RequestedSeatsNotAvailable();
        }
        flight.setSeatAvailable(flight.getSeatAvailable() - numberOfPassengers);
        flightRepository.save(flight);
        Passenger passenger1 = passengerRepository.save(passenger);
        reservation.setPassengerId(passenger1.getPassengerId());
        reservationRepository.save(reservation);
        sendMessage(reservation.toString(), topicArn, region);
        return true;

    }
}
```