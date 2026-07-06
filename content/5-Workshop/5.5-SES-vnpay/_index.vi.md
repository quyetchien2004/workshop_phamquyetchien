---
title : "SES email và VNPay sandbox"
date: 2026-07-06
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

## Mục tiêu

Phần này cấu hình hai tích hợp nghiệp vụ quan trọng của CCT Hotels Booking:

- **Amazon SES** để gửi OTP, email quên mật khẩu, xác nhận booking và mã khóa điện tử.
- **VNPay sandbox** để tạo URL thanh toán và nhận callback/IPN cập nhật trạng thái booking.

## 1. Cấu hình Amazon SES

SES được dùng ở region `ap-southeast-1`. Backend gửi email qua SMTP endpoint:

```text
email-smtp.ap-southeast-1.amazonaws.com
```

Các bước chính:

1. Vào Amazon SES > Verified identities.
2. Verify email gửi hoặc domain `viet70speed.xyz`.
3. Nếu verify domain, thêm 3 CNAME DKIM vào Namecheap.
4. Tạo SMTP credentials.
5. Cấu hình các biến `SMTP_*` trong Elastic Beanstalk.

Hiện tại SES vẫn đang ở Sandbox nên email chỉ gửi được giữa các địa chỉ đã verify. Trong phần demo, nhóm kiểm thử bằng email sender đã **Verified** để xác nhận chức năng gửi OTP hoạt động. Domain `viet70speed.xyz` còn **Unverified** do bước xác thực DKIM/DNS chưa hoàn tất, nên phần gửi email đại trà cho người dùng bất kỳ chưa được mở.

Hướng xử lý sau demo là hoàn tất DKIM cho domain và gửi yêu cầu production access để SES cho phép gửi ra ngoài danh sách email đã verify.

![Hình 33 - SES verified identity](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/1.png)
<p class="image-caption">Hình 33 - SES verified identity</p>

![Hình 34 - SES DKIM records ở Namecheap](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/2.png)
<p class="image-caption">Hình 34 - SES DKIM records ở Namecheap</p>

![Hình 35 - OTP email nhận được](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/3.png)
<p class="image-caption">Hình 35 - OTP email nhận được</p>

## 2. Cấu hình VNPay sandbox

Backend cần các biến môi trường:

```bash
VNPAY_TM_CODE=<tm-code>
VNPAY_HASH_SECRET=<hash-secret>
VNPAY_PAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
VNPAY_RETURN_URL=https://www.viet70speed.xyz/api/payments/vnpay/callback
VNPAY_IPN_URL=https://www.viet70speed.xyz/api/payments/vnpay/ipn
VNPAY_BANK_CODE=NCB
VNPAY_MOCK_ENABLED=false
```

Luồng thanh toán:

1. Người dùng tạo booking.
2. Backend tạo URL thanh toán VNPay đã ký bằng hash secret.
3. Trình duyệt chuyển sang VNPay sandbox.
4. Sau khi thanh toán, VNPay gọi Return URL/IPN về CloudFront.
5. CloudFront forward `/api/payments/vnpay/*` về backend.
6. Backend kiểm tra secure hash và cập nhật trạng thái booking/payment.

![Hình 36 - VNPay environment variables](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/4.png)
<p class="image-caption">Hình 36 - VNPay environment variables</p>

![Hình 37 - Trang thanh toán VNPay sandbox](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/5.png)
<p class="image-caption">Hình 37 - Trang thanh toán VNPay sandbox</p>

![Hình 38 - Kết quả thanh toán và trạng thái booking](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/6.png)
<p class="image-caption">Hình 38 - Kết quả thanh toán và trạng thái booking</p>

## 3. Lỗi thường gặp

| Lỗi | Nguyên nhân | Cách xử lý |
|---|---|---|
| SES báo email chưa verified | SES còn Sandbox hoặc email nhận chưa verify | Verify email nhận hoặc request production access |
| Không gửi được SMTP | Sai SMTP user/pass hoặc region | Kiểm tra SMTP credentials, `SMTP_HOST`, `SMTP_PORT` |
| VNPay báo website chưa phê duyệt | Sandbox merchant chưa được duyệt domain/return URL | Gửi VNPay domain, Return URL và IPN URL để duyệt |
| Callback không cập nhật booking | Sai Return URL/IPN hoặc CloudFront behavior `/api/*` | Kiểm tra `VNPAY_RETURN_URL`, `VNPAY_IPN_URL`, CloudFront và backend logs |



