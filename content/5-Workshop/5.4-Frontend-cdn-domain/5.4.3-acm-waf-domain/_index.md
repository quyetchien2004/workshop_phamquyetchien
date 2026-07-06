---
title : "Configure ACM, WAF and custom domain"
date: 2026-07-06
weight : 543
chapter : false
pre : " <b> 5.4.3 </b> "
---

## Objective

This step maps `viet70speed.xyz` and `www.viet70speed.xyz` to CloudFront, enables HTTPS using an ACM certificate, and attaches AWS WAF for edge-layer protection.

## 1. Create ACM certificate for CloudFront

CloudFront requires the ACM certificate to be created in region `us-east-1`. Create a certificate for:

```text
viet70speed.xyz
www.viet70speed.xyz
```

Choose DNS validation, then copy the generated CNAME validation records to Namecheap.

![Image 27 - ACM certificate Issued](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/1.png)
<p class="image-caption">Image 27 - ACM certificate Issued</p>

## 2. Add alternate domain names to CloudFront

In the CloudFront distribution, add:

```text
viet70speed.xyz
www.viet70speed.xyz
```

Then select the issued ACM certificate.

## 3. Configure DNS in Namecheap

In Namecheap > Advanced DNS, add a CNAME record:

| Type | Host | Value |
|---|---|---|
| CNAME | `www` | `d2r7fm91wsef8d.cloudfront.net` |

For the root domain `viet70speed.xyz`, depending on Namecheap support, use URL Redirect, ALIAS/ANAME if available, or use `www` as the main demo domain.

{{% notice note %}}
Do not delete ACM validation CNAME records. Deleting them may prevent automatic certificate renewal later.
{{% /notice %}}

![Image 28 - DNS records in Namecheap](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/2.png)
<p class="image-caption">Image 28 - DNS records in Namecheap</p>

## 4. Attach AWS WAF to CloudFront

Create a Web ACL for CloudFront with scope **CloudFront distributions**. Recommended managed rules:

- AWSManagedRulesCommonRuleSet
- AWSManagedRulesKnownBadInputsRuleSet
- AWSManagedRulesSQLiRuleSet
- Optional rate-based rule for unusual traffic spikes

![Image 29 - AWS WAF Web ACL](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.3-acm-waf-domain/3.png)
<p class="image-caption">Image 29 - AWS WAF Web ACL</p>



