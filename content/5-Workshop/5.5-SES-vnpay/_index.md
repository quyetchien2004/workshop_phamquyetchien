---
title : "SES email and VNPay sandbox"
date: 2026-07-06
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

## Objective

This section configures two important business integrations for CCT Hotels Booking:

- **Amazon SES** for OTP, password reset, booking confirmation, and electronic key emails.
- **VNPay sandbox** for payment URL creation and return/IPN callbacks that update booking status.

## 1. Configure Amazon SES

SES is used in region `ap-southeast-1`. The backend sends email through this SMTP endpoint:

```text
email-smtp.ap-southeast-1.amazonaws.com
```

Main steps:

1. Open Amazon SES > Verified identities.
2. Verify a sender email or the domain `viet70speed.xyz`.
3. If verifying the domain, add the three DKIM CNAME records to Namecheap.
4. Create SES SMTP credentials.
5. Configure `SMTP_*` variables in Elastic Beanstalk.

At this stage, SES is still in Sandbox, so email can only be sent to verified addresses. For the demo, the team tested OTP delivery with a **Verified** sender email. The domain `viet70speed.xyz` is still **Unverified** because DKIM/DNS verification has not been completed yet, so sending email to arbitrary users is not enabled.

The remaining step after the demo is to complete domain DKIM verification and request production access for SES.

![Image 33 - SES verified identity](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/1.png)
<p class="image-caption">Image 33 - SES verified identity</p>

![Image 34 - SES DKIM records in Namecheap](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/2.png)
<p class="image-caption">Image 34 - SES DKIM records in Namecheap</p>

![Image 35 - Received OTP email](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/3.png)
<p class="image-caption">Image 35 - Received OTP email</p>

## 2. Configure VNPay sandbox

The backend needs these environment variables:

```bash
VNPAY_TM_CODE=<tm-code>
VNPAY_HASH_SECRET=<hash-secret>
VNPAY_PAY_URL=https://sandbox.vnpayment.vn/paymentv2/vpcpay.html
VNPAY_RETURN_URL=https://www.viet70speed.xyz/api/payments/vnpay/callback
VNPAY_IPN_URL=https://www.viet70speed.xyz/api/payments/vnpay/ipn
VNPAY_BANK_CODE=NCB
VNPAY_MOCK_ENABLED=false
```

Payment flow:

1. User creates a booking.
2. Backend creates a signed VNPay payment URL.
3. Browser redirects to VNPay sandbox.
4. After payment, VNPay calls Return URL/IPN back to CloudFront.
5. CloudFront forwards `/api/payments/vnpay/*` to the backend.
6. Backend verifies the secure hash and updates booking/payment status.

![Image 36 - VNPay environment variables](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/4.png)
<p class="image-caption">Image 36 - VNPay environment variables</p>

![Image 37 - VNPay sandbox payment page](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/5.png)
<p class="image-caption">Image 37 - VNPay sandbox payment page</p>

![Image 38 - Payment result and booking status](/workshop_phamquyetchien/images/5-Workshop/5.5-SES-vnpay/6.png)
<p class="image-caption">Image 38 - Payment result and booking status</p>

## 3. Common issues

| Error | Cause | Fix |
|---|---|---|
| SES says email is not verified | SES Sandbox or unverified recipient | Verify recipient email or request production access |
| SMTP sending fails | Wrong SMTP user/password or region | Check SMTP credentials, `SMTP_HOST`, `SMTP_PORT` |
| VNPay says website is not approved | Sandbox merchant domain/return URL not approved | Send VNPay the domain, Return URL, and IPN URL for approval |
| Callback does not update booking | Wrong Return URL/IPN or CloudFront behavior | Check `VNPAY_RETURN_URL`, `VNPAY_IPN_URL`, CloudFront, and backend logs |



