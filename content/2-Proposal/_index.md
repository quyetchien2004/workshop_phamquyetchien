---
title: "Proposal"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, we summarize the proposed AWS deployment plan for the **CCT Hotels Booking** project.

# CCT Hotels Booking on AWS
## A Low-Cost Cloud Architecture for Hotel Booking, Payment, and Email Notification

### 1. Project Overview
CCT Hotels Booking is a web-based hotel booking system that supports customers and administrators in searching rooms, creating bookings, managing reservations, processing VNPay sandbox payments, and sending email notifications such as OTP codes, password reset emails, and booking confirmations.

The project is deployed on AWS with a cost-conscious architecture suitable for an internship report and demo environment. The frontend is hosted as a static website on Amazon S3 and delivered through Amazon CloudFront. The backend runs on Elastic Beanstalk with Node.js/Express and Socket.IO. Data is stored in Amazon DocumentDB, and transactional emails are sent through Amazon SES. VNPay is integrated as an external third-party payment gateway.

### 2. Objectives
- Deploy the CCT Hotels Booking web application to AWS in a clear and demonstrable architecture.
- Use AWS services that are practical for a student project while still reflecting real-world cloud deployment patterns.
- Separate frontend, backend, database, email, and payment responsibilities clearly.
- Improve performance and security by using CloudFront, HTTPS, WAF, private subnets, and security groups.
- Provide a stable demo flow: browse website, register/login, search rooms, create bookings, receive email notifications, and test VNPay sandbox payment.
- Prepare architecture documentation that can be reviewed easily by internship supervisors.

### 3. Problems to Solve
The original local deployment has several limitations:

- The application only runs on a local machine and cannot be accessed reliably by external users.
- Static frontend assets are not optimized for global delivery.
- Backend, database, and email configuration are tightly coupled in the local environment.
- Payment callback URLs cannot be tested properly without a public endpoint.
- Email sending through personal SMTP is not suitable for a cloud report or production-like architecture.
- Database access must be protected from the public Internet.
- The system needs clearer monitoring, security, and cost-control practices for AWS usage.

The proposed AWS architecture solves these issues by deploying each layer to the appropriate managed service and exposing only the required public entry points.

### 4. Solution Architecture
The solution uses a layered AWS architecture:

![CCT Hotels Booking AWS Architecture](/workshop_phamquyetchien/images/2-Proposal/anh%201.jpg)

#### Main AWS Services
- **Amazon S3**: Hosts the React/Vite production build and stores static frontend assets.
- **Amazon CloudFront**: Distributes the frontend through CDN, supports HTTPS, caching, and routes API traffic to the backend.
- **AWS WAF**: Protects the web entry point from common web attacks at the edge.
- **AWS Elastic Beanstalk**: Deploys and manages the Node.js/Express backend on EC2 with an Application Load Balancer.
- **Amazon DocumentDB**: Stores users, rooms, bookings, payments, OTP data, and application data using a MongoDB-compatible database.
- **Amazon SES**: Sends OTP codes, password reset messages, and booking confirmation emails.
- **Amazon Route 53 / Custom Domain**: Points the custom domain to CloudFront and supports DNS configuration.
- **AWS Certificate Manager (ACM)**: Provides HTTPS certificates for CloudFront.
- **Amazon VPC**: Separates public and private resources using subnets, route tables, security groups, NAT Gateway, and Internet Gateway.

#### Architecture Flow
1. Users access the website through the custom domain or CloudFront domain over HTTPS.
2. CloudFront serves static frontend files from the private S3 bucket.
3. AWS WAF filters common malicious requests before they reach the application.
4. API requests such as `/api/*` and Socket.IO requests such as `/socket.io/*` are routed from CloudFront to Elastic Beanstalk.
5. Elastic Beanstalk forwards traffic through the Application Load Balancer to the Node.js backend.
6. The backend connects privately to Amazon DocumentDB for application data.
7. Amazon DocumentDB stores booking, account, payment, room, and OTP-related data.
8. The backend sends email through Amazon SES SMTP.
9. For payment, the backend creates a VNPay payment URL and redirects the customer to the VNPay sandbox gateway.
10. VNPay returns the payment result to the configured return URL and IPN endpoint through CloudFront and the backend API.

#### Network and Security Design
- Public subnets contain public-facing resources such as the load balancer and NAT Gateway.
- Private subnets contain application instances and Amazon DocumentDB.
- DocumentDB is not exposed to the Internet and only accepts traffic from the application security group.
- Application instances use NAT Gateway for outbound Internet access when needed.
- CloudFront uses HTTPS and ACM certificates.
- WAF is attached at the CloudFront layer.
- Environment variables are configured in Elastic Beanstalk and secrets are not committed to the repository.

### 5. Timeline
| Phase | Duration | Main Tasks | Deliverables |
|---|---:|---|---|
| Planning | Week 9 | Hold group meeting, choose CCT Hotels Booking topic, split tasks, draw AWS architecture on draw.io | Project topic, AWS architecture diagram, team responsibility split |
| Network | Week 10 | Create VPC, public/private subnets, route tables, Internet Gateway, NAT Gateway, and main security groups | Network environment ready for frontend, backend, and database |
| Backend / Database | Week 10 - Week 11 | Deploy Node.js/Express backend, configure ALB, target group, health check, and connect Amazon DocumentDB | Backend URL, health check API, working database connection |
| Frontend / CDN | Week 10 - Week 11 | Build frontend, upload to S3, create CloudFront distribution, attach WAF/HTTPS/domain, and configure API behavior | Public website URL, CloudFront routing, HTTPS access |
| Integration / Test | Week 11 | Test register/login, room search, booking, email, VNPay sandbox, and frontend API flow | Project main flow works, screenshots and report evidence prepared |

### 6. Budget Estimation
This project uses a low-cost AWS setup for demo and internship reporting. Costs depend on running time, region, traffic, and whether resources are stopped after testing.

| Component | Estimated Cost Notes |
|---|---|
| Amazon S3 | Very low cost for static frontend storage |
| Amazon CloudFront | Low cost for demo traffic; may fit within free usage depending on account eligibility |
| AWS WAF | Additional monthly and request-based cost if enabled |
| Elastic Beanstalk / EC2 | Cost depends on instance type and uptime |
| Application Load Balancer | Adds hourly and traffic-based cost |
| Amazon DocumentDB | Main cost driver, especially when the instance runs continuously |
| NAT Gateway | Can become expensive if left running for a long time |
| Amazon SES | Very low cost for small email volume |
| Route 53 / Domain | Domain and hosted zone costs if used |

For a short demo, the cost can be controlled by stopping or deleting nonessential resources after use. If all resources are left running for a full month, the expected cost may be significantly higher, mainly because of DocumentDB, NAT Gateway, EC2, and Load Balancer usage. AWS Budgets should be configured to alert the team when spending approaches the planned limit.

### 7. Risks
| Risk | Impact | Mitigation |
|---|---|---|
| Unexpected AWS cost | High | Use AWS Budgets, stop DocumentDB/Elastic Beanstalk/NAT Gateway after demo, monitor Billing Dashboard |
| SES sandbox limitation | Medium | Verify sender and recipient emails or request production access before wider testing |
| VNPay sandbox approval issue | Medium | Use correct sandbox credentials, return URL, IPN URL, and contact VNPay support if the site is not approved |
| CORS errors after deployment | Medium | Configure backend `CLIENT_URL` and `SERVER_URL` with the CloudFront/custom domain |
| CloudFront cache serving old files | Low | Create invalidation for `/*` after each frontend upload |
| DocumentDB connection failure | High | Check VPC, private subnet group, security group rule, TLS CA file, and MongoDB URI |
| Security group misconfiguration | High | Allow only required inbound ports and restrict database access to the app security group |
| Secrets exposed in repository or screenshots | High | Store secrets in environment variables, rotate exposed credentials, avoid committing `.env` |
| Single-instance demo limitation | Medium | Accept for demo cost optimization or scale to multiple instances for high availability |

### 8. Expected Outcome
After implementation, the system will provide a complete cloud-based hotel booking demo with public frontend access, backend API deployment, protected database access, transactional email, and VNPay sandbox payment integration. The architecture is suitable for an internship project because it demonstrates practical AWS services, deployment workflow, security design, and cost-control considerations.
