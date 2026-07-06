---
title: "Worklog Tuần 10"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Triển khai các dịch vụ AWS chính cho project **CCT Hotels Booking** dựa trên sơ đồ kiến trúc đã vẽ ở tuần 9.
* Phân chia công việc rõ ràng trong nhóm:
  * Quyền: Frontend S3, CloudFront, AWS WAF.
  * Em: Network và chuẩn bị phần Backend + ALB.
  * Tiến Huy: DocumentDB, SES và VNPay.
* Hoàn thành phần network gồm VPC, subnet, route table, internet gateway và NAT Gateway.
* Chuẩn bị xong môi trường mạng để các phần backend, database và frontend có thể kết nối đúng.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 6 | - Họp nhóm để chốt nhiệm vụ triển khai CCT Hotels Booking <br> - Xác nhận em phụ trách phần Network và Backend + ALB | 19/06/2026 | 19/06/2026 | |
| 2 | - Tạo cấu trúc VPC theo sơ đồ <br> - Chuẩn bị public subnet và private subnet cho 2 Availability Zones | 22/06/2026 | 22/06/2026 | |
| 3 | - Cấu hình internet gateway và route table cho public subnet <br> - Kiểm tra hướng đi internet cho các tài nguyên public | 23/06/2026 | 23/06/2026 | |
| 4 | - Tìm hiểu và chuẩn bị NAT Gateway cho private subnet <br> - Xem lại luồng kết nối từ backend private subnet ra internet khi cần pull package hoặc gọi service ngoài | 24/06/2026 | 24/06/2026 | |
| 5 | - Kiểm tra lại sơ đồ network với nhóm <br> - Hoàn tất phần network để chuyển sang triển khai backend và ALB | 25/06/2026 | 25/06/2026 | |

### Kết quả đạt được tuần 10:

* Nhóm đã chốt nhiệm vụ rõ ràng cho từng thành viên.
* Em nắm phần mình cần làm là Network và Backend + ALB.
* Hoàn thành phần chuẩn bị network chính cho hệ thống CCT Hotels Booking.
* Xác định được public subnet dùng cho ALB/NAT Gateway và private subnet dùng cho backend server, DocumentDB.
* Hiểu hơn vai trò của VPC, subnet, route table, internet gateway và NAT Gateway trong kiến trúc thực tế.
* Sau tuần này, phần network của project đã hoàn thành và sẵn sàng cho bước hoàn thiện backend, ALB và test toàn hệ thống.
