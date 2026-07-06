---
title: "Worklog Tuần 11"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Hoàn thiện project **CCT Hotels Booking** sau khi đã chuẩn bị network ở tuần 10.
* Hoàn thành phần em phụ trách: Backend + Application Load Balancer.
* Phối hợp với Quyền để frontend gọi được API qua CloudFront/ALB.
* Phối hợp với Tiến Huy để backend kết nối được DocumentDB và chuẩn bị luồng SES/VNPay.
* Cùng nhóm test và hoàn thành project.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 6 | - Kiểm tra lại network đã chuẩn bị ở tuần 10 <br> - Xác định subnet, security group và port cần dùng cho backend và ALB | 26/06/2026 | 26/06/2026 | |
| 2 | - Triển khai backend Node.js/Express API lên EC2 hoặc Elastic Beanstalk theo sơ đồ <br> - Kiểm tra biến môi trường cơ bản cho backend | 29/06/2026 | 29/06/2026 | |
| 3 | - Cấu hình Application Load Balancer cho backend <br> - Tạo target group và kiểm tra health check cho backend server | 30/06/2026 | 30/06/2026 | |
| 4 | - Phối hợp với Tiến Huy để kiểm tra kết nối backend đến DocumentDB <br> - Kiểm tra security group giữa backend và database | 01/07/2026 | 01/07/2026 | |
| 5 | - Cùng nhóm test luồng frontend gọi API qua CloudFront/ALB <br> - Kiểm tra các lỗi kết nối, chỉnh cấu hình và chốt project đã chạy được luồng chính | 02/07/2026 | 02/07/2026 | |

### Kết quả đạt được tuần 11:

* Backend đã được triển khai xong theo kiến trúc của CCT Hotels Booking.
* Application Load Balancer đã được cấu hình xong để nhận traffic API/Socket.IO từ CloudFront.
* Em hiểu rõ hơn cách ALB kết nối với backend server thông qua target group và health check.
* Backend đã kết nối được với DocumentDB theo phần Tiến Huy triển khai.
* Nhóm đã test luồng frontend gọi API qua CloudFront/ALB và xử lý các lỗi cấu hình chính.
* Sau tuần này, project CCT Hotels Booking đã hoàn thành được luồng chính theo sơ đồ kiến trúc.
