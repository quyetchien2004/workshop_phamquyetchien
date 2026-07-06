---
title : "Tổng quan workshop"
date: 2026-07-06
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

## Tổng quan hệ thống

**CCT Hotels Booking** là website đặt phòng khách sạn gồm frontend React/Vite và backend Node.js/Express. Người dùng có thể xem danh sách phòng, đăng ký, đăng nhập, đặt phòng, nhận OTP qua email và thanh toán bằng VNPay sandbox. Admin có thể quản lý người dùng, phòng, booking, mã khóa điện tử và các yêu cầu hỗ trợ.

Khi triển khai lên AWS, nhóm chia hệ thống thành ba lớp chính:

- **Lớp truy cập người dùng**: Route 53/Namecheap DNS, CloudFront, WAF và S3 frontend.
- **Lớp backend**: CloudFront forward `/api/*` và `/socket.io/*` về ALB/Elastic Beanstalk.
- **Lớp dữ liệu và tích hợp**: DocumentDB lưu dữ liệu, SES gửi email, VNPay xử lý thanh toán sandbox.

## Kiến trúc triển khai

1. Người dùng truy cập `https://www.viet70speed.xyz` hoặc CloudFront domain.
2. CloudFront trả về file frontend tĩnh từ S3.
3. Request `/api/*` và `/socket.io/*` được CloudFront chuyển về backend Elastic Beanstalk.
4. ALB chuyển request đến EC2 App Server chạy Node.js port `8080` trong private subnet.
5. Backend đọc/ghi dữ liệu trong Amazon DocumentDB qua port `27017`.
6. Backend gọi Amazon SES để gửi OTP, reset password, thông báo booking và mã khóa điện tử.
7. Backend tạo URL thanh toán VNPay; VNPay callback/IPN quay về CloudFront rồi vào backend.

## Kết quả cần đạt

- Website mở được qua CloudFront hoặc domain riêng.
- API `/api/health` trả về trạng thái healthy.
- Frontend gọi API production thay vì `localhost`.
- Đăng ký, đăng nhập và tạo booking hoạt động.
- Backend kết nối được DocumentDB.
- SES gửi được email OTP hoặc email hệ thống.
- VNPay sandbox tạo được trang thanh toán và callback về hệ thống.

![Hình 01 - Sơ đồ kiến trúc tổng thể](/workshop_phamquyetchien/images/5-Workshop/5.1-Workshop-overview/anh%201.jpg)
<p class="image-caption">Hình 01 - Sơ đồ kiến trúc tổng thể</p>



