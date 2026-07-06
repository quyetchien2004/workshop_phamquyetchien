---
title : "Network, backend and database"
date: 2026-07-06
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

## Objective

This section deploys the backend and database foundation for CCT Hotels Booking. The backend runs on Elastic Beanstalk in private subnets and receives traffic through ALB/CloudFront. The database uses Amazon DocumentDB and only allows access from the App Server security group through port `27017`.

## Network design

The VPC `cct-hotels-vpc` uses CIDR `10.0.0.0/16` and spans two Availability Zones:

- Public subnets A/B: Application Load Balancer and NAT Gateway.
- Private subnets A/B: Elastic Beanstalk EC2 App Server and Amazon DocumentDB.
- Internet Gateway: attached to the VPC so public subnets can access the Internet.
- NAT Gateway: allows backend instances in private subnets to call external services such as SES and VNPay.

## Backend request flow

1. CloudFront receives `/api/*` or `/socket.io/*` requests.
2. CloudFront forwards the request to ALB/Elastic Beanstalk.
3. ALB forwards the request to the EC2 App Server on port `8080`.
4. The App Server connects to DocumentDB through internal security group rules.
5. The App Server sends email through SES and creates VNPay payment URLs.

![Image 05 - VPC resource map](/images/5-Workshop/5.3-Network-backend-database/1.png)
<p class="image-caption">Image 05 - VPC resource map</p>

![Image 06 - Public and private route tables](/images/5-Workshop/5.3-Network-backend-database/2.png)
<p class="image-caption">Image 06 - Public and private route tables</p>



