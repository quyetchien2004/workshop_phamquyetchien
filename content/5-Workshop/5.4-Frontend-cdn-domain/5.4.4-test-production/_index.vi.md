---
title : "Kiểm thử website production"
date: 2026-07-06
weight : 544
chapter : false
pre : " <b> 5.4.4 </b> "
---

## Mục tiêu

Sau khi frontend, CloudFront, WAF và domain đã cấu hình xong, cần kiểm thử website ở domain production thay vì local.

## 1. Mở website bằng domain riêng

Truy cập:

```text
https://www.viet70speed.xyz
```

Kiểm tra các trang chính:

- Trang chủ.
- Danh sách phòng.
- Trang đăng nhập/đăng ký.
- Trang đặt phòng.
- Trang admin nếu có tài khoản admin.

![Hình 30 - Trang chủ production](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/1.png)
<p class="image-caption">Hình 30 - Trang chủ production</p>

## 2. Kiểm tra API từ frontend

Mở DevTools > Network, thực hiện login hoặc gọi danh sách phòng. Request API phải gọi về:

```text
https://www.viet70speed.xyz/api/...
```

Không được gọi `localhost`.

![Hình 31 - Frontend gọi API production](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/2.png)
<p class="image-caption">Hình 31 - Frontend gọi API production</p>

## 3. Kiểm tra route SPA

Thử refresh trực tiếp các đường dẫn:

```text
https://www.viet70speed.xyz/login
https://www.viet70speed.xyz/room
https://www.viet70speed.xyz/my-bookings
```

Nếu không bị 403/404, CloudFront SPA fallback đã hoạt động.

## 4. Kiểm tra HTTPS

Trình duyệt phải hiển thị kết nối HTTPS hợp lệ. Khi bấm vào biểu tượng ổ khóa, certificate phải thuộc domain `viet70speed.xyz` hoặc `www.viet70speed.xyz`.

![Hình 32 - HTTPS certificate trên domain](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/3.png)
<p class="image-caption">Hình 32 - HTTPS certificate trên domain</p>



