---
title: "Frontend, CDN, WAF và domain"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

Phần này triển khai frontend React/Vite lên Amazon S3, phân phối qua CloudFront, bật WAF ở edge và gắn domain riêng `viet70speed.xyz`.

Luồng truy cập:

```text
User -> Namecheap DNS -> CloudFront + WAF -> S3 frontend
                              |
                              +-> /api/*, /socket.io/* -> Elastic Beanstalk backend
```

Các thành phần chính:

- S3 bucket: `cct-hotels-frontend-bucket`
- CloudFront distribution: `cct-hotels-frontend-cdn`
- CloudFront domain: `d2r7fm91wsef8d.cloudfront.net`
- Custom domain: `viet70speed.xyz`, `www.viet70speed.xyz`
- ACM certificate: tạo ở `us-east-1` để dùng cho CloudFront
- WAF Web ACL: gắn vào CloudFront

![Hình 18 - S3 bucket frontend](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/1.png)
<p class="image-caption">Hình 18 - S3 bucket frontend</p>

![Hình 19 - CloudFront distribution](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/2.png)
<p class="image-caption">Hình 19 - CloudFront distribution</p>



