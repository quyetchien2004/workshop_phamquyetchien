---
title: "Worklog Tuần 7"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu networking cơ bản trong AWS.
* Làm quen với VPC, subnet, route table và internet gateway.
* Hiểu vì sao network quan trọng khi triển khai ứng dụng trên cloud.
* Làm theo một bài lab tạo VPC để hiểu các thành phần mạng chính.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 6 | - Tìm hiểu VPC là gì <br> - Tìm hiểu khái niệm network riêng trong AWS | 29/05/2026 | 29/05/2026 | <https://docs.aws.amazon.com/vpc/> |
| 2 | - Tìm hiểu subnet public và private ở mức cơ bản <br> - Tìm hiểu sự khác nhau khi tài nguyên có hoặc không có internet access | 01/06/2026 | 01/06/2026 | <https://docs.aws.amazon.com/vpc/> |
| 3 | - Tìm hiểu route table và internet gateway <br> - Xem vai trò của route trong việc điều hướng traffic | 02/06/2026 | 02/06/2026 | <https://docs.aws.amazon.com/vpc/> |
| 4 | - Tìm hiểu security group và network ACL ở mức giới thiệu <br> - So sánh đơn giản hai lớp kiểm soát traffic này | 03/06/2026 | 03/06/2026 | <https://docs.aws.amazon.com/vpc/> |
| 5 | - Làm lab tạo VPC cơ bản <br> - Vẽ lại sơ đồ đơn giản gồm VPC, subnet, route table và internet gateway | 04/06/2026 | 04/06/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/create-vpc.html> |

### Kết quả đạt được tuần 7:

* Hiểu VPC là môi trường mạng riêng để đặt tài nguyên AWS.
* Biết subnet dùng để chia nhỏ network trong VPC.
* Hiểu sơ bộ public subnet có thể kết nối internet nếu cấu hình đúng route và internet gateway.
* Biết route table quyết định traffic đi theo hướng nào.
* Hiểu security group liên quan đến traffic của tài nguyên như EC2.
* Sau bài lab tạo VPC, em dễ hình dung hơn việc các subnet và route table liên kết với nhau.
* Tuần này khá khó hơn các tuần trước vì networking có nhiều khái niệm mới, nhưng em đã nắm được bức tranh tổng quát.
