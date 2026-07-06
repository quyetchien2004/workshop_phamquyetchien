---
title: "Tạo VPC, Security Group, DocumentDB và Elastic Beanstalk"
date: 2026-07-06
weight: 531
chapter: false
pre: " <b> 5.3.1 </b> "
---

Ở bước này, nhóm tạo hạ tầng cho backend và database. Mục tiêu là chỉ public những thành phần cần thiết như CloudFront/ALB, còn App Server và DocumentDB nằm trong private subnet.

## 1. Tạo VPC và subnet

Tạo VPC:

- Name: `cct-hotels-vpc`
- IPv4 CIDR: `10.0.0.0/16`
- Region: `ap-southeast-1`

Tạo 4 subnet:

| Subnet | AZ | CIDR gợi ý | Mục đích |
|---|---|---|---|
| Public subnet A | ap-southeast-1a | `10.0.1.0/24` | ALB, NAT Gateway A |
| Public subnet B | ap-southeast-1b | `10.0.2.0/24` | ALB, NAT Gateway B |
| Private subnet A | ap-southeast-1a | `10.0.3.0/24` | App Server, DocumentDB primary |
| Private subnet B | ap-southeast-1b | `10.0.4.0/24` | App Server, DocumentDB replica/reader |

## 2. Tạo Internet Gateway và NAT Gateway

- Internet Gateway attach vào `cct-hotels-vpc`.
- Public route table trỏ `0.0.0.0/0` về Internet Gateway.
- NAT Gateway đặt trong public subnet.
- Private route table trỏ `0.0.0.0/0` về NAT Gateway.

Cách này giúp EC2 backend trong private subnet không bị public trực tiếp, nhưng vẫn có thể gọi ra Internet khi cần gửi email SES, gọi VNPay hoặc tải package.

## 3. Tạo Security Group

| Security group | Inbound rule | Mục đích |
|---|---|---|
| `cct-alb-sg` | HTTP `80`, HTTPS `443` từ Internet/CloudFront | Cho ALB nhận request |
| `cct-app-sg` | TCP `8080` từ `cct-alb-sg` | Cho ALB gọi Node.js backend |
| `cct-docdb-sg` | TCP `27017` từ `cct-app-sg` | Chỉ backend được kết nối database |

Không mở DocumentDB cho `0.0.0.0/0`.

![Hình 07 - Security Group của ALB](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/1.png)
<p class="image-caption">Hình 07 - Security Group của ALB</p>

![Hình 08 - Security Group của App Server](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/2.png)
<p class="image-caption">Hình 08 - Security Group của App Server</p>

![Hình 09 - Security Group của DocumentDB](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/3.png)
<p class="image-caption">Hình 09 - Security Group của DocumentDB</p>

## 4. Tạo Amazon DocumentDB

Tạo cluster:

- Cluster identifier: `cct-hotels-docdb`
- Engine: Amazon DocumentDB MongoDB-compatible
- Port: `27017`
- Subnet group: gồm private subnet A và private subnet B
- Publicly accessible: No
- Security group: `cct-docdb-sg`
- Username: `cctadmin`
- Database dùng trong app: `hotel_booking`

Chuỗi kết nối mẫu:

```text
mongodb://cctadmin:<password>@cct-hotels-docdb.cluster-xxxxx.ap-southeast-1.docdb.amazonaws.com:27017/hotel_booking?tls=true&tlsCAFile=global-bundle.pem&replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false
```

Khi deploy, file `global-bundle.pem` phải nằm trong source bundle để backend kết nối TLS tới DocumentDB.

![Hình 10 - DocumentDB cluster Available](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/4.png)
<p class="image-caption">Hình 10 - DocumentDB cluster Available</p>

## 5. Deploy backend bằng Elastic Beanstalk

Tạo Elastic Beanstalk:

- Application: `cct-hotels-backend`
- Environment: `cct-hotels-backend-env-v2`
- Platform: Node.js on Amazon Linux 2023
- Environment type: Load balanced
- App port: `8080`
- Health check path: `/api/health`
- Instance subnet: private subnet A/B
- Load balancer subnet: public subnet A/B
- EC2 security group: `cct-app-sg`

Source bundle cần có `Procfile`:

```text
web: npm run start --workspace backend
```

![Hình 11 - Elastic Beanstalk configuration](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/5.png)
<p class="image-caption">Hình 11 - Elastic Beanstalk configuration</p>

![Hình 12 - Elastic Beanstalk environment variables](/images/5-Workshop/5.3-Network-backend-database/5.3.1-create-infrastructure/6.png)
<p class="image-caption">Hình 12 - Elastic Beanstalk environment variables</p>



