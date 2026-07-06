---
title : "Chuẩn bị môi trường"
date: 2026-07-06
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

## Mục tiêu

Trước khi triển khai CCT Hotels Booking lên AWS, nhóm cần chuẩn bị tài khoản, source code, domain và các thông tin cấu hình quan trọng. Phần này giúp người đọc biết trước những tài nguyên nào sẽ được dùng trong các bước sau.

## Tài khoản và phân công nhóm

Nhóm sử dụng một AWS account chung cho đồ án. Không thao tác bằng root account trong quá trình làm hằng ngày; mỗi thành viên nên có IAM user riêng.

| Thành viên | Phần phụ trách | Dịch vụ chính |
|---|---|---|
| Người 1 | Frontend, CDN, domain, bảo mật edge | S3, CloudFront, WAF, ACM, Namecheap |
| Người 2 | Network và backend | VPC, subnet, route table, SG, ALB, Elastic Beanstalk |
| Người 3 | Database, email, payment | DocumentDB, SES, VNPay |

## Thông tin project

- Frontend: React/Vite.
- Backend: Node.js/Express.
- Backend port: `8080`.
- Health check path: `/api/health`.
- Database: Amazon DocumentDB, MongoDB-compatible.
- File CA cho DocumentDB: `global-bundle.pem`.
- Domain: `viet70speed.xyz` và `www.viet70speed.xyz`.

## Tài nguyên AWS đã dùng

| Thành phần | Tên/Giá trị |
|---|---|
| Region chính | `ap-southeast-1` Singapore |
| VPC | `cct-hotels-vpc`, CIDR `10.0.0.0/16` |
| S3 frontend bucket | `cct-hotels-frontend-bucket` |
| CloudFront distribution | `cct-hotels-frontend-cdn` |
| CloudFront domain | `d2r7fm91wsef8d.cloudfront.net` |
| Elastic Beanstalk app | `cct-hotels-backend` |
| Elastic Beanstalk env | `cct-hotels-backend-env-v2` |
| DocumentDB cluster | `cct-hotels-docdb` |
| SES SMTP endpoint | `email-smtp.ap-southeast-1.amazonaws.com` |
| VNPay mode | Sandbox |

## Biến môi trường backend

```bash
NODE_ENV=production
PORT=8080
AWS_REGION=ap-southeast-1
CLIENT_URL=https://www.viet70speed.xyz,https://viet70speed.xyz,https://d2r7fm91wsef8d.cloudfront.net
SERVER_URL=https://www.viet70speed.xyz
MONGO_URI=mongodb://cctadmin:<password>@cct-hotels-docdb.cluster-xxxxx.ap-southeast-1.docdb.amazonaws.com:27017/hotel_booking?tls=true&tlsCAFile=global-bundle.pem&replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false
JWT_SECRET=<jwt-secret>
SMTP_HOST=email-smtp.ap-southeast-1.amazonaws.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_FROM_NAME=CCT Hotels Company
SMTP_FROM_EMAIL=<email-da-verify-trong-SES>
SMTP_USER=<ses-smtp-user>
SMTP_PASS=<ses-smtp-password>
VNPAY_TM_CODE=<vnpay-tm-code>
VNPAY_HASH_SECRET=<vnpay-hash-secret>
VNPAY_PAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
VNPAY_RETURN_URL=https://www.viet70speed.xyz/api/payments/vnpay/callback
VNPAY_IPN_URL=https://www.viet70speed.xyz/api/payments/vnpay/ipn
VNPAY_BANK_CODE=NCB
VNPAY_MOCK_ENABLED=false
```

{{% notice warning %}}
Không đưa password thật, SMTP password, JWT secret hoặc VNPay hash secret vào báo cáo. Khi chụp màn hình cần che hoặc làm mờ các giá trị nhạy cảm.
{{% /notice %}}

![Hình 03 - IAM users của nhóm](/images/5-Workshop/5.2-Prerequisite/anh%20aim%20use.png)
<p class="image-caption">Hình 03 - IAM users của nhóm</p>

![Hình 04 - Domain Namecheap](/images/5-Workshop/5.2-Prerequisite/domain.png)
<p class="image-caption">Hình 04 - Domain Namecheap</p>



