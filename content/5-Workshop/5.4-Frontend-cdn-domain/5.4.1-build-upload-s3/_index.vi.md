---
title: "Build frontend và upload lên Amazon S3"
date: 2026-07-06
weight: 541
chapter: false
pre: " <b> 5.4.1 </b> "
---

## Mục tiêu

Ở bước này, nhóm build frontend React/Vite và upload nội dung trong thư mục `frontend/dist` lên S3. S3 chỉ lưu file tĩnh, còn CloudFront sẽ đứng phía trước để phục vụ HTTPS và cache.

## 1. Cấu hình biến môi trường frontend

Trong thư mục `frontend`, tạo hoặc cập nhật file `.env.production`:

```env
VITE_API_URL=https://www.viet70speed.xyz/api
VITE_SOCKET_URL=https://www.viet70speed.xyz
```

Nếu chưa dùng domain riêng, có thể dùng CloudFront domain:

```env
VITE_API_URL=https://d2r7fm91wsef8d.cloudfront.net/api
VITE_SOCKET_URL=https://d2r7fm91wsef8d.cloudfront.net
```

Không dùng `localhost` trong bản production.

![Hình 20 - File env production của frontend](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/1.png)
<p class="image-caption">Hình 20 - File env production của frontend</p>

## 2. Build frontend

Chạy lệnh:

```bash
npm run build --workspace frontend
```

Hoặc nếu đang đứng trong thư mục `frontend`:

```bash
npm run build
```

Sau khi build thành công, thư mục `frontend/dist` sẽ có `index.html` và thư mục `assets/`.

![Hình 21 - Build frontend thành công](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/2.png)
<p class="image-caption">Hình 21 - Build frontend thành công</p>

## 3. Upload lên S3

Vào AWS Console > S3:

1. Chọn bucket `cct-hotels-frontend-bucket`.
2. Bấm **Upload**.
3. Chọn toàn bộ file/thư mục **bên trong** `frontend/dist`, không upload nguyên thư mục `dist`.
4. Bấm **Upload**.

![Hình 22 - Upload frontend dist lên S3](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/3.png)
<p class="image-caption">Hình 22 - Upload frontend dist lên S3</p>

## 4. Tạo CloudFront invalidation

Sau mỗi lần upload bản frontend mới, cần xóa cache CloudFront:

1. Vào CloudFront > Distributions.
2. Chọn `cct-hotels-frontend-cdn`.
3. Mở tab **Invalidations**.
4. Chọn **Create invalidation**.
5. Nhập object path: `/*`.

![Hình 23 - CloudFront invalidation](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/4.png)
<p class="image-caption">Hình 23 - CloudFront invalidation</p>



