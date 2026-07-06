---
title: "Week 11 Worklog"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Finish the **CCT Hotels Booking** project after preparing the network in week 10.
* Complete my part: Backend + Application Load Balancer.
* Work with Quyen so the frontend can call APIs through CloudFront/ALB.
* Work with Tien Huy so the backend can connect to DocumentDB and prepare SES/VNPay flows.
* Test and complete the project with the group.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Friday | - Checked the network prepared in week 10 <br> - Identified subnets, security groups, and ports needed for backend and ALB | 26/06/2026 | 26/06/2026 | |
| Monday | - Deployed the Node.js/Express backend API to EC2 or Elastic Beanstalk based on the diagram <br> - Checked basic environment variables for the backend | 29/06/2026 | 29/06/2026 | |
| Tuesday | - Configured Application Load Balancer for the backend <br> - Created target group and checked health check for backend servers | 30/06/2026 | 30/06/2026 | |
| Wednesday | - Worked with Tien Huy to check backend connection to DocumentDB <br> - Checked security group between backend and database | 01/07/2026 | 01/07/2026 | |
| Thursday | - Tested frontend calling API through CloudFront/ALB with the group <br> - Checked connection errors, adjusted configuration, and confirmed the main flow worked | 02/07/2026 | 02/07/2026 | |

### Week 11 Achievements:

* The backend was completed following the CCT Hotels Booking architecture.
* Application Load Balancer was configured to receive API/Socket.IO traffic from CloudFront.
* I understood better how ALB connects to backend servers through target group and health check.
* The backend connected to DocumentDB from Tien Huy's part.
* The group tested the frontend API flow through CloudFront/ALB and fixed the main configuration issues.
* After this week, the CCT Hotels Booking project completed the main flow based on the architecture diagram.
