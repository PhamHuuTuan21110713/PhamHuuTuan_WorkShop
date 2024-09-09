---
title : "Chạy thử API với Postman"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

- Qua các phần trên, chúng ta đã tạo thành công các API (tìm chuyến bay và đặt chỗ chuyến bay ) và đã thử nghiệm sự hoạt động của nó thông qua các test case, và dường như chúng đều chạy rất ổn. Nhưng để thực tế , chúng ta cần phải có các request thực tới API của chúng ta để có thểm kiểm chứng độ hoạt động của API. 
- Trong phần này, chúng ta sẽ tiến hành thử nghiệm lần lượt với cả 2 API với Postman để xem API của chúng ta có hoạt động hay không. Nếu thành công chúng ta có thể nhận được các phản hồi tương ứng theo từng module.
### 1. Với API đặt chuyến bay (Module 2)
- Bây giờ chúng ta sẽ sử dụng API của chúng ta để đặt thử 1 chuyến bay, khi mà chúng ta đặt thì chúng ta sẽ có e-mail báo đặt thành công, và dữ liệu cũng sẽ được thêm vào database.
- Đầu tiên chúng ta cần chạy được dự án của chúng ta lên, chúng ta mở tệp **FlightBookingApplication** lên và chạy nó.

![image.png](/images/test_postman/image.png)


- Tiếp theo chúng ta vào **Postman**, chúng ta sẽ kết nối tới **[http://localhost:8090/reserve](http://localhost:8090/reserve)**. Chúng ta dùng phương thức Post.

![image.png](/images/test_postman/image_1.png)

- Tiếp theo, chúng ta vào phần **Authorization. Type** chọn là **No auth.**

![image.png](/images/test_postman/image_2.png)

- Sau đó chúng ta qua phần **Body**, chọn **raw**, sau đó dán vào dữ liệu **Json** như sau.


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

- Tiếp theo qua tab **Headers**, thêm vào 1 **key** là **Authorization**, và **value** sẽ là **access_token** mà chúng ta có khi đăng nhập qua **Cognito**.

![image.png](/images/test_postman/image_4.png)

- Cuối cùng bấm nút **Send**

![image.png](/images/test_postman/image_5.png)

- Chúng ta sẽ xem kết quả, response sẽ có nội dung như sau.

![image.png](/images/test_postman/image_6.png)

- Trước tiên chúng ta sẽ nhận được 1 mail thông báo thành công.

![image.png](/images/test_postman/image_7.png)

- Và dữ liệu cũng sẽ được thêm vào 2 bảng (**passenger** và **reservation**) ứng với nội dung lệnh **Json** của chúng ta.

Bảng **reservation**

![image.png](/images/test_postman/image_8.png)

Bảng **passenger**

![image.png](/images/test_postman/image_9.png)

##### => Đạt yêu cầu cho module 2

### 2. Đối với API Tìm chuyến bay (Module 1)

- Cũng với các thao tác tương tự, chúng ta cũng vào Postman, chúng ta thay đổi phương thức thành GET, sau đó thay đổi URL như dưới đây, chúng ta có thể thay đổi nội dung tìm kiếm.

![image.png](/images/test_postman/image_10.png)

- Và đây là kết quả trả về

![image.png](/images/test_postman/image_11.png)
##### => Đạt yêu cầu cho module 1

Vậy cả 2 API của chúng ta tạo ra đều hoạt động tốt, đều trả về response tương ứng.