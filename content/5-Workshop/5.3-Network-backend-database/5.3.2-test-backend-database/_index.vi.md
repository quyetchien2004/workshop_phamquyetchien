---
title: "Kiểm thử backend và database"
date: 2026-07-06
weight: 532
chapter: false
pre: " <b> 5.3.2 </b> "
---

Sau khi deploy backend, nhóm kiểm thử theo thứ tự từ hạ tầng đến nghiệp vụ.

## 1. Kiểm tra trạng thái Elastic Beanstalk

Môi trường `cct-hotels-backend-env-v2` cần có trạng thái:

- Health: `Ok`
- Platform: Node.js on Amazon Linux 2023
- App port: `8080`
- Health check path: `/api/health`

![Hình 13 - Elastic Beanstalk Health OK](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/1.png)
<p class="image-caption">Hình 13 - Elastic Beanstalk Health OK</p>

## 2. Kiểm tra API trực tiếp qua Elastic Beanstalk

Mở trình duyệt:

```text
http://<elastic-beanstalk-domain>/api/health
```

Kết quả mong đợi:

```json
{
  "status": "ok",
  "message": "Backend healthy"
}
```

![Hình 14 - Backend health qua Elastic Beanstalk](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/2.png)
<p class="image-caption">Hình 14 - Backend health qua Elastic Beanstalk</p>

## 3. Kiểm tra API qua CloudFront

Sau khi CloudFront có behavior `/api/*`, kiểm thử:

```text
https://d2r7fm91wsef8d.cloudfront.net/api/health
```

Hoặc domain riêng:

```text
https://www.viet70speed.xyz/api/health
```

Nếu trả về JSON healthy, CloudFront đã forward API về backend đúng.

![Hình 15 - Backend health qua CloudFront](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/3.png)
<p class="image-caption">Hình 15 - Backend health qua CloudFront</p>

## 4. Kiểm thử kết nối DocumentDB

Kiểm thử qua các thao tác có đọc/ghi database:

- Đăng ký tài khoản.
- Đăng nhập.
- Xem danh sách phòng.
- Tạo booking thử.
- Vào admin xem booking mới.

Nếu các thao tác này lưu và đọc dữ liệu được, backend đã kết nối DocumentDB thành công.

![Hình 16 - Đăng ký hoặc đăng nhập thành công](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/4.png)
<p class="image-caption">Hình 16 - Đăng ký hoặc đăng nhập thành công</p>

![Hình 17 - Booking được lưu trong hệ thống](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/5.png)
<p class="image-caption">Hình 17 - Booking được lưu trong hệ thống</p>

## 5. Lỗi thường gặp

| Lỗi | Nguyên nhân thường gặp | Cách xử lý |
|---|---|---|
| `Not allowed by CORS` | `CLIENT_URL` chưa có domain frontend | Thêm CloudFront/domain riêng vào `CLIENT_URL`, apply EB config |
| CloudFront `/api/health` lỗi 504 | Origin backend dùng HTTPS trong khi EB đang HTTP | Sửa CloudFront origin protocol sang HTTP only hoặc match viewer phù hợp |
| EB health Severe | Sai port, sai health path hoặc app crash | Kiểm tra `PORT=8080`, `/api/health`, EB logs |
| DocumentDB connect fail | Sai SG, URI, subnet hoặc CA bundle | Kiểm tra `cct-docdb-sg`, `MONGO_URI`, `global-bundle.pem` |


