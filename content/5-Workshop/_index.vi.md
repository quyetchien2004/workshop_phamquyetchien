---
title : "Workshop"
date: 2026-07-06
weight : 5
chapter : true
pre : " <b> 5. </b> "
---

# Workshop triển khai CCT Hotels Booking trên AWS

Phần workshop này ghi lại quá trình nhóm triển khai hệ thống **CCT Hotels Booking** lên AWS. Nội dung được viết theo đúng các bước đã làm trong đồ án: đưa frontend React/Vite lên S3 và CloudFront, triển khai backend Node.js/Express bằng Elastic Beanstalk, kết nối Amazon DocumentDB, cấu hình Amazon SES để gửi OTP/email và tích hợp VNPay sandbox.

Mục tiêu của workshop là chứng minh hệ thống có thể chạy được trên môi trường cloud, có domain/HTTPS, có backend API, có database riêng, có email giao dịch và có luồng thanh toán thử nghiệm.

Các nhóm dịch vụ chính:

1. **Frontend/CDN**: Amazon S3, Amazon CloudFront, AWS WAF, ACM, Namecheap DNS.
2. **Network/Backend**: VPC, public/private subnet, Internet Gateway, NAT Gateway, Security Group, ALB, Elastic Beanstalk.
3. **Database/Email/Payment**: Amazon DocumentDB, Amazon SES SMTP, VNPay sandbox.
4. **Kiểm thử và vận hành**: health check, CORS, CloudFront behavior, invalidation, cleanup tài nguyên.

## Content

1. [Workshop overview](5.1-workshop-overview/)
2. [Prerequisite](5.2-prerequiste/)
3. [Network, backend and database](5.3-network-backend-database/)
4. [Frontend, CDN and domain](5.4-frontend-cdn-domain/)
5. [SES and VNPay](5.5-ses-vnpay/)
6. [Final test and cleanup](5.6-final-test-cleanup/)


