---
title : "Configure CloudFront origins and behaviors"
date: 2026-07-06
weight : 542
chapter : false
pre : " <b> 5.4.2 </b> "
---

## Objective

CloudFront has two roles in this deployment: serving static frontend files from S3 and forwarding API/Socket.IO traffic to the Elastic Beanstalk backend.

## 1. Create the S3 origin

In CloudFront distribution `cct-hotels-frontend-cdn`, create an origin that points to S3 bucket `cct-hotels-frontend-bucket`. If the bucket is private, use **Origin Access Control (OAC)** so CloudFront can read files without making the bucket public.

## 2. Create the Elastic Beanstalk backend origin

Create another origin that points to the Elastic Beanstalk domain, for example:

```text
cct-hotels-backend-env-v2.eba-xxxxx.ap-southeast-1.elasticbeanstalk.com
```

Because the Elastic Beanstalk environment currently serves HTTP, set the origin protocol to `HTTP only`. If CloudFront uses HTTPS only while the EB origin does not support HTTPS, requests may fail with `504 Gateway Timeout`.

![Image 24 - CloudFront origins](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/1.png)
<p class="image-caption">Image 24 - CloudFront origins</p>

## 3. Create behaviors

| Path pattern | Origin | Purpose |
|---|---|---|
| `/api/*` | Elastic Beanstalk backend | Express API |
| `/socket.io/*` | Elastic Beanstalk backend | Socket.IO realtime traffic |
| Default `*` | S3 frontend | React/Vite static files |

For `/api/*` and `/socket.io/*`, forward the required query strings, headers, and cookies if authentication or realtime communication needs them.

![Image 25 - CloudFront behaviors](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/2.png)
<p class="image-caption">Image 25 - CloudFront behaviors</p>

## 4. Configure SPA fallback

Because the frontend is a React SPA, refreshing routes such as `/login`, `/room`, or `/my-bookings` can return 403/404 from S3. Configure custom error responses:

| HTTP error | Response page path | HTTP response code |
|---|---|---|
| 403 | `/index.html` | 200 |
| 404 | `/index.html` | 200 |

![Image 26 - CloudFront custom error response](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/3.png)
<p class="image-caption">Image 26 - CloudFront custom error response</p>



