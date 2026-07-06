---
title: "Frontend, CDN, WAF and domain"
date: 2026-07-06
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

This section deploys the React/Vite frontend to Amazon S3, distributes it through CloudFront, enables edge protection with AWS WAF, and maps the custom domain `viet70speed.xyz`.

Access flow:

```text
User -> Namecheap DNS -> CloudFront + WAF -> S3 frontend
                              |
                              +-> /api/*, /socket.io/* -> Elastic Beanstalk backend
```

Main components:

- S3 bucket: `cct-hotels-frontend-bucket`
- CloudFront distribution: `cct-hotels-frontend-cdn`
- CloudFront domain: `d2r7fm91wsef8d.cloudfront.net`
- Custom domains: `viet70speed.xyz`, `www.viet70speed.xyz`
- ACM certificate: created in `us-east-1` for CloudFront
- WAF Web ACL: attached to CloudFront

![Image 18 - S3 frontend bucket](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/1.png)
<p class="image-caption">Image 18 - S3 frontend bucket</p>

![Image 19 - CloudFront distribution](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/2.png)
<p class="image-caption">Image 19 - CloudFront distribution</p>



