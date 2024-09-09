---
title : "Clean up"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
- Chúng ta tiến hành xóa đi **Stack** mà chúng ta đã nạp file **.yaml** của Workshop cung cấp.
- Vào **CloudFormation,** vào mục **Stacks** và tick chọn vào **stack** mà lúc đó chúng ta tạo cho **Workshop.**

![image.png](/images/clean_up/image.png)

- Sau đó chúng ta bấm **Delete**, và chờ cho quá trình xóa hoàn thành, nó sẽ tự động xóa các tài nguyên liên quan. Chẳng hạn như EC2 instance mà chúng ta dùng trong suốt quá trình làm Lab thì đã tự động bị terminate.

![image.png](/images/clean_up/image_1.png)