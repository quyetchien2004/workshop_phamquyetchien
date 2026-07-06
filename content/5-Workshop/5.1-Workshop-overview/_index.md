---
title : "Workshop overview"
date: 2026-07-06
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

## System overview

**CCT Hotels Booking** is a hotel booking web application with a React/Vite frontend and a Node.js/Express backend. Users can browse rooms, register, log in, make bookings, receive OTP emails, and pay through VNPay sandbox. Admin users can manage users, rooms, bookings, electronic lock codes, and support requests.

On AWS, the system is divided into three main layers:

- **User access layer**: Route 53/Namecheap DNS, CloudFront, WAF, and S3 frontend hosting.
- **Backend layer**: CloudFront forwards `/api/*` and `/socket.io/*` traffic to ALB/Elastic Beanstalk.
- **Data and integration layer**: DocumentDB stores application data, SES sends transactional emails, and VNPay handles sandbox payment testing.

## Deployment architecture

1. Users access `https://www.viet70speed.xyz` or the CloudFront domain.
2. CloudFront serves static frontend files from Amazon S3.
3. Requests to `/api/*` and `/socket.io/*` are forwarded to the Elastic Beanstalk backend.
4. ALB forwards requests to EC2 App Server instances running Node.js on port `8080` in private subnets.
5. The backend reads and writes data to Amazon DocumentDB through port `27017`.
6. The backend uses Amazon SES to send OTP, password reset, booking notification, and electronic key emails.
7. The backend creates a VNPay payment URL; VNPay return/IPN callbacks are routed back through CloudFront to the backend.

## Expected results

After completing the workshop:

- The website is accessible through CloudFront or the custom domain.
- `/api/health` returns a healthy backend response.
- The production frontend calls the production API instead of `localhost`.
- Registration, login, and booking creation work correctly.
- The backend connects to Amazon DocumentDB.
- SES can send OTP or system emails.
- VNPay sandbox can create a payment page and callback to the system.

![Image 01 - Overall architecture diagram](/workshop_phamquyetchien/images/5-Workshop/5.1-Workshop-overview/anh%201.jpg)
<p class="image-caption">Image 01 - Overall architecture diagram</p>



