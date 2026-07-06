---
title : "Kiểm thử cuối và dọn dẹp tài nguyên"
date: 2026-07-06
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

## Mục tiêu

Sau khi hoàn tất triển khai, nhóm cần kiểm thử end-to-end và dọn dẹp các tài nguyên có thể phát sinh chi phí cao. Phần này cũng dùng làm checklist cuối cho báo cáo.

## 1. Checklist kiểm thử cuối

Kiểm thử các chức năng chính:

- Truy cập website qua `https://www.viet70speed.xyz`.
- API `/api/health` hoạt động qua CloudFront/domain.
- Đăng ký tài khoản mới.
- Đăng nhập.
- Xem danh sách phòng.
- Tạo booking.
- Nhận email OTP hoặc email hệ thống qua SES.
- Thanh toán thử bằng VNPay sandbox.
- Admin xem/duyệt booking.
- Socket.IO hoặc thông báo realtime nếu có dùng trong trang admin.

![Hình 39 - Kịch bản kiểm thử end-to-end](/workshop_phamquyetchien/images/5-Workshop/5.6-Final-test-cleanup/1.png)
<p class="image-caption">Hình 39 - Kịch bản kiểm thử end-to-end</p>

## 2. Theo dõi log khi có lỗi

Các nơi cần kiểm tra log:

- Elastic Beanstalk > Logs: lỗi backend start, CORS, env vars.
- CloudFront > Monitoring: lỗi 4xx/5xx.
- SES > Sending statistics: bounce/complaint.
- DocumentDB > Cluster events hoặc CloudWatch metrics.
- Browser DevTools > Network: lỗi API từ frontend.

![Hình 40 - Elastic Beanstalk logs hoặc health](/workshop_phamquyetchien/images/5-Workshop/5.6-Final-test-cleanup/2.png)
<p class="image-caption">Hình 40 - Elastic Beanstalk logs hoặc health</p>

## 3. Dọn dẹp tài nguyên để tránh phát sinh chi phí

Khi demo xong hoặc không còn sử dụng, cần dọn các tài nguyên tốn phí:

1. Elastic Beanstalk environment, EC2 instances, ALB.
2. NAT Gateway và Elastic IP của NAT Gateway.
3. Amazon DocumentDB cluster nếu không cần giữ dữ liệu.
4. CloudFront distribution nếu không dùng nữa.
5. S3 bucket frontend và bucket upload nếu không cần lưu.
6. WAF Web ACL nếu không còn gắn CloudFront.
7. Route 53 hosted zone hoặc record nếu có dùng Route 53.
8. SES identity/SMTP user nếu muốn khóa lại sau demo.

{{% notice warning %}}
DocumentDB, NAT Gateway, ALB và EC2 là các thành phần dễ phát sinh chi phí. Nếu chỉ làm báo cáo/demo, nên stop/delete đúng lúc và kiểm tra Billing Dashboard.
{{% /notice %}}

![Hình 41 - AWS Billing hoặc danh sách tài nguyên cần dọn](/workshop_phamquyetchien/images/5-Workshop/5.6-Final-test-cleanup/3.png)
<p class="image-caption">Hình 41 - AWS Billing hoặc danh sách tài nguyên cần dọn</p>

## 4. Kết luận workshop

Qua workshop này, nhóm đã triển khai được một hệ thống đặt phòng khách sạn full-stack lên AWS với frontend, backend, database, email và thanh toán sandbox. Kiến trúc vẫn tối ưu chi phí cho đồ án, nhưng đã có đầy đủ các thành phần quan trọng của một hệ thống web production cơ bản: CDN, HTTPS, private backend/database, security group, email giao dịch và callback thanh toán.


