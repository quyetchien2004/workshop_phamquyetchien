---
title : "Cấu hình CloudFront origins và behaviors"
date: 2026-07-06
weight : 542
chapter : false
pre : " <b> 5.4.2 </b> "
---

## Mục tiêu

CloudFront đóng hai vai trò trong hệ thống: phục vụ frontend tĩnh từ S3 và forward các request API/Socket.IO về backend Elastic Beanstalk.

## 1. Tạo origin S3 cho frontend

Trong CloudFront distribution `cct-hotels-frontend-cdn`, tạo origin trỏ đến S3 bucket `cct-hotels-frontend-bucket`. Nếu bucket để private, nên dùng **Origin Access Control (OAC)** để CloudFront được quyền đọc file trong S3 mà không cần public bucket.

## 2. Tạo origin backend Elastic Beanstalk

Tạo thêm origin trỏ đến domain Elastic Beanstalk, ví dụ:

```text
cct-hotels-backend-env-v2.eba-xxxxx.ap-southeast-1.elasticbeanstalk.com
```

Vì backend Elastic Beanstalk đang chạy HTTP, origin protocol nên để `HTTP only`. Nếu để HTTPS only trong khi EB domain chưa có HTTPS, CloudFront có thể trả lỗi `504 Gateway Timeout`.

![Hình 24 - CloudFront origins](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/1.png)
<p class="image-caption">Hình 24 - CloudFront origins</p>

## 3. Tạo behaviors

| Path pattern | Origin | Mục đích |
|---|---|---|
| `/api/*` | Elastic Beanstalk backend | API Express |
| `/socket.io/*` | Elastic Beanstalk backend | Socket.IO realtime |
| Default `*` | S3 frontend | React/Vite static files |

Với `/api/*` và `/socket.io/*`, nên forward query string và các header/cookie cần thiết nếu hệ thống dùng auth hoặc realtime.

![Hình 25 - CloudFront behaviors](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/2.png)
<p class="image-caption">Hình 25 - CloudFront behaviors</p>

## 4. Cấu hình SPA fallback

Vì frontend là React SPA, khi refresh các route như `/login`, `/room`, `/my-bookings`, CloudFront/S3 có thể trả 403 hoặc 404. Cần cấu hình custom error response:

| HTTP error | Response page path | HTTP response code |
|---|---|---|
| 403 | `/index.html` | 200 |
| 404 | `/index.html` | 200 |

![Hình 26 - CloudFront custom error response](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.2-cloudfront-behaviors/3.png)
<p class="image-caption">Hình 26 - CloudFront custom error response</p>



