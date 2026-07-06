---
title : "Final test and cleanup"
date: 2026-07-06
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

## Objective

After finishing the deployment, run end-to-end tests and clean up resources that may generate high cost. This section also works as the final checklist for the report.

## 1. Final test checklist

Test the main features:

- Access the website through `https://www.viet70speed.xyz`.
- `/api/health` works through CloudFront/custom domain.
- Register a new account.
- Log in.
- Browse rooms.
- Create a booking.
- Receive OTP or system email through SES.
- Complete a VNPay sandbox payment.
- Admin can view/approve bookings.
- Socket.IO or realtime notifications work if used by the admin page.

![Image 39 - End-to-end test result](/images/5-Workshop/5.6-Final-test-cleanup/1.png)
<p class="image-caption">Image 39 - End-to-end test result</p>

## 2. Check logs when issues happen

Useful places to inspect:

- Elastic Beanstalk > Logs: backend startup, CORS, environment variable errors.
- CloudFront > Monitoring: 4xx/5xx errors.
- SES > Sending statistics: bounce/complaint.
- DocumentDB > Cluster events or CloudWatch metrics.
- Browser DevTools > Network: frontend API errors.

![Image 40 - Elastic Beanstalk logs or health](/images/5-Workshop/5.6-Final-test-cleanup/2.png)
<p class="image-caption">Image 40 - Elastic Beanstalk logs or health</p>

## 3. Clean up resources to reduce cost

After demo or when the system is no longer needed, clean up expensive resources:

1. Elastic Beanstalk environment, EC2 instances, ALB.
2. NAT Gateway and NAT Gateway Elastic IP.
3. Amazon DocumentDB cluster if data does not need to be retained.
4. CloudFront distribution if no longer used.
5. S3 frontend/upload buckets if files do not need to be kept.
6. WAF Web ACL if it is no longer attached to CloudFront.
7. Route 53 hosted zone or records if used.
8. SES identities/SMTP users if they should be disabled after demo.

{{% notice warning %}}
DocumentDB, NAT Gateway, ALB, and EC2 are the most likely resources to generate noticeable cost. For a demo/report project, stop or delete them when not in use and check the Billing Dashboard.
{{% /notice %}}

![Image 41 - AWS Billing or cleanup check](/images/5-Workshop/5.6-Final-test-cleanup/3.png)
<p class="image-caption">Image 41 - AWS Billing or cleanup check</p>

## 4. Workshop conclusion

In this workshop, the team deployed a full-stack hotel booking system to AWS with frontend hosting, backend API, private database, transactional email, and sandbox payment. The architecture is cost-aware for an internship project but still includes key production-style components: CDN, HTTPS, private backend/database, security groups, transactional email, and payment callbacks.


