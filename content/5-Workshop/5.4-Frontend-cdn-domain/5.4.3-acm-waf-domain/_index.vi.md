---
title : "Cấu hình ACM, WAF và domain riêng"
date: 2026-07-06
weight : 543
chapter : false
pre : " <b> 5.4.3 </b> "
---

## Mục tiêu

Ở bước này, nhóm gắn domain `viet70speed.xyz` và `www.viet70speed.xyz` vào CloudFront, bật HTTPS bằng ACM certificate và gắn AWS WAF để tăng bảo vệ ở lớp edge.

## 1. Tạo ACM certificate cho CloudFront

CloudFront yêu cầu ACM certificate nằm ở region `us-east-1`. Tạo certificate cho:

```text
viet70speed.xyz
www.viet70speed.xyz
```

Chọn DNS validation, sau đó copy các CNAME validation record sang Namecheap.

![Hình 27 - ACM certificate Issued](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/1.png)
<p class="image-caption">Hình 27 - ACM certificate Issued</p>

## 2. Thêm alternate domain names vào CloudFront

Trong CloudFront distribution, thêm:

```text
viet70speed.xyz
www.viet70speed.xyz
```

Sau đó chọn ACM certificate vừa issued.

## 3. Cấu hình DNS ở Namecheap

Trong Namecheap > Advanced DNS, thêm CNAME:

| Type | Host | Value |
|---|---|---|
| CNAME | `www` | `d2r7fm91wsef8d.cloudfront.net` |

Với root domain `viet70speed.xyz`, tùy cấu hình Namecheap có thể dùng URL Redirect, ALIAS/ANAME nếu được hỗ trợ, hoặc dùng `www` làm domain chính trong báo cáo/demo.

{{% notice note %}}
Không xóa các CNAME validation của ACM. Nếu xóa, certificate có thể mất khả năng tự gia hạn sau này.
{{% /notice %}}

![Hình 28 - DNS records ở Namecheap](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/2.png)
<p class="image-caption">Hình 28 - DNS records ở Namecheap</p>

## 4. Gắn AWS WAF vào CloudFront

Tạo Web ACL cho CloudFront, scope chọn **CloudFront distributions**. Có thể bật các managed rules cơ bản:

- AWSManagedRulesCommonRuleSet
- AWSManagedRulesKnownBadInputsRuleSet
- AWSManagedRulesSQLiRuleSet
- Rate-based rule nếu muốn giới hạn request bất thường

![Hình 29 - AWS WAF Web ACL](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/3.png)
<p class="image-caption">Hình 29 - AWS WAF Web ACL</p>



