---
title : "Workshop"
date: 2026-07-06
weight : 5
chapter : true
pre : " <b> 5. </b> "
---

# Workshop: Deploying CCT Hotels Booking on AWS

This workshop documents the AWS deployment process of **CCT Hotels Booking**. The project uses a React/Vite frontend, a Node.js/Express backend, Amazon DocumentDB for MongoDB-compatible storage, Amazon SES for transactional emails, and VNPay sandbox for payment testing.

The goal of this workshop is to show that the application can run in a cloud environment with HTTPS, CDN delivery, a private backend/database layer, email delivery, and a sandbox payment flow.

Main service groups:

1. **Frontend/CDN**: Amazon S3, Amazon CloudFront, AWS WAF, ACM, and Namecheap DNS.
2. **Network/Backend**: VPC, public/private subnets, Internet Gateway, NAT Gateway, Security Groups, ALB, and Elastic Beanstalk.
3. **Database/Email/Payment**: Amazon DocumentDB, Amazon SES SMTP, and VNPay sandbox.
4. **Testing/Operations**: health check, CORS, CloudFront behaviors, invalidation, logs, and resource cleanup.

## Content

1. [Workshop overview](5.1-workshop-overview/)
2. [Prerequisite](5.2-prerequiste/)
3. [Network, backend and database](5.3-network-backend-database/)
4. [Frontend, CDN and domain](5.4-frontend-cdn-domain/)
5. [SES and VNPay](5.5-ses-vnpay/)
6. [Final test and cleanup](5.6-final-test-cleanup/)


