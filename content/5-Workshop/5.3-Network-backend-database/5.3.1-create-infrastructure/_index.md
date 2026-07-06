---
title: "Create VPC, security groups, DocumentDB and Elastic Beanstalk"
date: 2026-07-06
weight: 531
chapter: false
pre: " <b> 5.3.1 </b> "
---

This step creates the infrastructure for the backend and database layer. Only CloudFront/ALB should be publicly reachable. The App Server and DocumentDB remain in private subnets.

## 1. Create VPC and subnets

Create the VPC:

- Name: `cct-hotels-vpc`
- IPv4 CIDR: `10.0.0.0/16`
- Region: `ap-southeast-1`

Create four subnets:

| Subnet | AZ | Suggested CIDR | Purpose |
|---|---|---|---|
| Public subnet A | ap-southeast-1a | `10.0.1.0/24` | ALB, NAT Gateway A |
| Public subnet B | ap-southeast-1b | `10.0.2.0/24` | ALB, NAT Gateway B |
| Private subnet A | ap-southeast-1a | `10.0.3.0/24` | App Server, DocumentDB primary |
| Private subnet B | ap-southeast-1b | `10.0.4.0/24` | App Server, DocumentDB replica/reader |

## 2. Create Internet Gateway and NAT Gateway

- Attach an Internet Gateway to `cct-hotels-vpc`.
- Route public subnet traffic `0.0.0.0/0` to the Internet Gateway.
- Place the NAT Gateway in a public subnet.
- Route private subnet traffic `0.0.0.0/0` to the NAT Gateway.

This keeps backend EC2 instances private while still allowing them to reach external services such as SES, VNPay, or package repositories.

## 3. Create security groups

| Security group | Inbound rule | Purpose |
|---|---|---|
| `cct-alb-sg` | HTTP `80`, HTTPS `443` from Internet/CloudFront | Allows ALB to receive requests |
| `cct-app-sg` | TCP `8080` from `cct-alb-sg` | Allows ALB to call Node.js backend |
| `cct-docdb-sg` | TCP `27017` from `cct-app-sg` | Allows only backend to access database |

Do not expose DocumentDB to `0.0.0.0/0`.

![Image 07 - ALB security group](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/1.png)
<p class="image-caption">Image 07 - ALB security group</p>

![Image 08 - App Server security group](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/2.png)
<p class="image-caption">Image 08 - App Server security group</p>

![Image 09 - DocumentDB security group](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/3.png)
<p class="image-caption">Image 09 - DocumentDB security group</p>

## 4. Create Amazon DocumentDB

Create the cluster:

- Cluster identifier: `cct-hotels-docdb`
- Engine: Amazon DocumentDB, MongoDB-compatible
- Port: `27017`
- Subnet group: private subnet A and private subnet B
- Publicly accessible: No
- Security group: `cct-docdb-sg`
- Username: `cctadmin`
- Application database name: `hotel_booking`

Sample connection string:

```text
mongodb://cctadmin:<password>@cct-hotels-docdb.cluster-xxxxx.ap-southeast-1.docdb.amazonaws.com:27017/hotel_booking?tls=true&tlsCAFile=global-bundle.pem&replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false
```

The `global-bundle.pem` file must be included in the backend source bundle so Node.js can establish a TLS connection to DocumentDB.

![Image 10 - DocumentDB cluster Available](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/4.png)
<p class="image-caption">Image 10 - DocumentDB cluster Available</p>

## 5. Deploy backend with Elastic Beanstalk

Create Elastic Beanstalk:

- Application: `cct-hotels-backend`
- Environment: `cct-hotels-backend-env-v2`
- Platform: Node.js on Amazon Linux 2023
- Environment type: Load balanced
- App port: `8080`
- Health check path: `/api/health`
- Instance subnets: private subnet A/B
- Load balancer subnets: public subnet A/B
- EC2 security group: `cct-app-sg`

The source bundle should include this `Procfile`:

```text
web: npm run start --workspace backend
```

![Image 11 - Elastic Beanstalk configuration](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/5.png)
<p class="image-caption">Image 11 - Elastic Beanstalk configuration</p>

![Image 12 - Elastic Beanstalk environment variables](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/6.png)
<p class="image-caption">Image 12 - Elastic Beanstalk environment variables</p>



