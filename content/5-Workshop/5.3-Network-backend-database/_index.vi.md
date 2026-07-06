---
title : "Network, backend và database"
date: 2026-07-06
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

## Mục tiêu

Phần này triển khai nền tảng chạy backend và database cho CCT Hotels Booking. Backend được đưa lên Elastic Beanstalk, chạy trong private subnet và nhận request thông qua ALB/CloudFront. Database dùng Amazon DocumentDB và chỉ cho phép App Server truy cập qua port `27017`.

## Thiết kế network

VPC `cct-hotels-vpc` dùng CIDR `10.0.0.0/16`, chia thành hai Availability Zone:

- Public subnet A/B: đặt Application Load Balancer và NAT Gateway.
- Private subnet A/B: đặt EC2 App Server của Elastic Beanstalk và DocumentDB.
- Internet Gateway: gắn với VPC để public subnet có đường ra Internet.
- NAT Gateway: cho private subnet đi ra Internet khi backend cần gọi SES, VNPay hoặc cập nhật package.

## Luồng request backend

1. CloudFront nhận request `/api/*` hoặc `/socket.io/*`.
2. CloudFront chuyển request đến ALB/Elastic Beanstalk backend.
3. ALB forward request vào EC2 App Server port `8080`.
4. App Server kết nối DocumentDB qua security group nội bộ.
5. App Server gọi SES để gửi email và gọi VNPay để tạo URL thanh toán.

![Hình 05 - VPC resource map](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/1.png)
<p class="image-caption">Hình 05 - VPC resource map</p>

![Hình 06 - Route tables public/private](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/2.png)
<p class="image-caption">Hình 06 - Route tables public/private</p>



