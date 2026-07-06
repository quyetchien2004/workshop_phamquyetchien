---
title: "Week 10 Worklog"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Deploy the main AWS services for the **CCT Hotels Booking** project based on the architecture diagram from week 9.
* Split the group tasks clearly:
  * Quyen: Frontend S3, CloudFront, AWS WAF.
  * Me: Network and preparation for Backend + ALB.
  * Tien Huy: DocumentDB, SES, and VNPay.
* Complete the network part, including VPC, subnets, route tables, internet gateway, and NAT Gateway.
* Finish the network environment so backend, database, and frontend parts can connect correctly.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Friday | - Had a group meeting to finalize deployment tasks for CCT Hotels Booking <br> - Confirmed that I was responsible for Network and Backend + ALB | 19/06/2026 | 19/06/2026 | |
| Monday | - Created the VPC structure based on the architecture diagram <br> - Prepared public subnets and private subnets for 2 Availability Zones | 22/06/2026 | 22/06/2026 | |
| Tuesday | - Configured internet gateway and route table for public subnets <br> - Checked internet routing for public resources | 23/06/2026 | 23/06/2026 | |
| Wednesday | - Prepared NAT Gateway for private subnets <br> - Checked how backend instances in private subnets can access the internet when needed | 24/06/2026 | 24/06/2026 | |
| Thursday | - Reviewed the network diagram with the group <br> - Completed the network part before moving to backend and ALB deployment | 25/06/2026 | 25/06/2026 | |

### Week 10 Achievements:

* The group finalized the deployment tasks clearly.
* I understood that my main parts were Network and Backend + ALB.
* I completed the main network preparation for CCT Hotels Booking.
* I identified public subnets for ALB/NAT Gateway and private subnets for backend servers and DocumentDB.
* I understood better the roles of VPC, subnet, route table, internet gateway, and NAT Gateway in a real architecture.
* After this week, the network part of the project was completed and ready for backend, ALB, and system testing.
