---
title : "Prerequisites"
date: 2026-07-06
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

## Objective

Before deploying CCT Hotels Booking to AWS, the team needs to prepare the AWS account, source code, domain, and deployment configuration values. This section lists the resources and values used throughout the workshop.

## AWS account and team roles

The team uses one shared AWS account for the project. The root account should not be used for daily operations; each member should have a separate IAM user.

| Member | Responsibility | Main services |
|---|---|---|
| Member 1 | Frontend, CDN, domain, edge security | S3, CloudFront, WAF, ACM, Namecheap |
| Member 2 | Network and backend deployment | VPC, subnets, route tables, SG, ALB, Elastic Beanstalk |
| Member 3 | Database, email, payment | DocumentDB, SES, VNPay |

## Project information

- Frontend: React/Vite.
- Backend: Node.js/Express.
- Backend port: `8080`.
- Health check path: `/api/health`.
- Database: Amazon DocumentDB, MongoDB-compatible.
- DocumentDB CA file: `global-bundle.pem`.
- Domain: `viet70speed.xyz` and `www.viet70speed.xyz`.

## AWS resources used

| Component | Name/Value |
|---|---|
| Main region | `ap-southeast-1` Singapore |
| VPC | `cct-hotels-vpc`, CIDR `10.0.0.0/16` |
| S3 frontend bucket | `cct-hotels-frontend-bucket` |
| CloudFront distribution | `cct-hotels-frontend-cdn` |
| CloudFront domain | `d2r7fm91wsef8d.cloudfront.net` |
| Elastic Beanstalk application | `cct-hotels-backend` |
| Elastic Beanstalk environment | `cct-hotels-backend-env-v2` |
| DocumentDB cluster | `cct-hotels-docdb` |
| SES SMTP endpoint | `email-smtp.ap-southeast-1.amazonaws.com` |
| VNPay mode | Sandbox |

## Backend environment variables

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
SMTP_FROM_EMAIL=<ses-verified-email-or-domain>
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
Do not include real passwords, SMTP passwords, JWT secrets, or VNPay hash secrets in the public report. Sensitive values should be blurred in screenshots.
{{% /notice %}}

![Image 03 - IAM users](/workshop_phamquyetchien/images/5-Workshop/5.2-Prerequisite/anh%20aim%20use.png)
<p class="image-caption">Image 03 - IAM users</p>

![Image 04 - Namecheap domain](/workshop_phamquyetchien/images/5-Workshop/5.2-Prerequisite/domain.png)
<p class="image-caption">Image 04 - Namecheap domain</p>



