---
title : "Test API with Postman"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

- Through the above sections, we have successfully created APIs (find flights and book flights) and tested their working through test cases, and it seems that they all work fine. But for real, we need to have real requests to our API to be able to verify the working of the API.
- In this section, we will test both APIs with Postman to see if our APIs work or not. If successful, we can receive corresponding responses for each module.
### 1. With Flight Reservation API (Module 2)
- Now we will use our API to book a test flight, when we book we will get an email notification of successful booking, and the data will also be added to the database.
- First we need to run our project, we open the **FlightBookingApplication** file and run it.

![image.png](/images/test_postman/image.png)


- Next we go to **Postman**, we will connect to **[http://localhost:8090/reserve](http://localhost:8090/reserve)**. We use the Post method.

![image.png](/images/test_postman/image_1.png)

- Next, we go to **Authorization. Type** select **No auth.**

![image.png](/images/test_postman/image_2.png)

- Then we go to **Body**, select **raw**, then paste in the **Json** data as follows.


```
 {
    "firstName":"John",
    "lastName":"Davis",
    "gender":"Male",
    "age": 10,
    "flightId": 1,
    "travelClass":"Economy",
    "ticketPrice":"100",
    "currencyCode":"USD",
    "paymentMode":"CC",
    "contactNumber": "0928895717",
    "contactEmail" : "phamhuutuanaws@gmail.com"
}
```


![image.png](/images/test_postman/image_3.png)

- Next, go to the **Headers** tab, add a **key** which is **Authorization**, and the **value** will be the **access_token** that we have when logging in via **Cognito**.

![image.png](/images/test_postman/image_4.png)

- Finally click the **Send** button

![image.png](/images/test_postman/image_5.png)

- We will see the result, the response will have the following content.

![image.png](/images/test_postman/image_6.png)

- First we will receive a success notification email.

![image.png](/images/test_postman/image_7.png)

- And the data will also be added to 2 tables (**passenger** and **reservation**) corresponding to the content of our **Json** command.

**reservation** table

![image.png](/images/test_postman/image_8.png)

**passenger** table

![image.png](/images/test_postman/image_9.png)

##### =>Meets requirements for module 2

### 2. With Find Flights API (Module 1)

- With the same operations, we also go to Postman, we change the method to GET, then change the URL as below, we can change the search content.

![image.png](/images/test_postman/image_10.png)

- And here is the result returned

![image.png](/images/test_postman/image_11.png)
##### => Meets requirements for module 1

So both of our APIs work fine and return the corresponding response.