---
title : "Test production website"
date: 2026-07-06
weight : 544
chapter : false
pre : " <b> 5.4.4 </b> "
---

## Objective

After configuring frontend hosting, CloudFront, WAF, and domain mapping, test the website through the production domain instead of local development URLs.

## 1. Open the custom domain

Visit:

```text
https://www.viet70speed.xyz
```

Check the main pages:

- Home page.
- Room list.
- Login/Register pages.
- Booking page.
- Admin pages if an admin account is available.

![Image 30 - Production homepage](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/1.png)
<p class="image-caption">Image 30 - Production homepage</p>

## 2. Check frontend API calls

Open DevTools > Network, then log in or load the room list. API requests should go to:

```text
https://www.viet70speed.xyz/api/...
```

They should not call `localhost`.

![Image 31 - Frontend calls production API](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/2.png)
<p class="image-caption">Image 31 - Frontend calls production API</p>

## 3. Test SPA routes

Refresh these routes directly:

```text
https://www.viet70speed.xyz/login
https://www.viet70speed.xyz/room
https://www.viet70speed.xyz/my-bookings
```

If they do not return 403/404, the CloudFront SPA fallback is working.

## 4. Check HTTPS certificate

The browser should show a valid HTTPS connection. When clicking the lock icon, the certificate should be issued for `viet70speed.xyz` or `www.viet70speed.xyz`.

![Image 32 - HTTPS certificate on domain](/workshop_phamquyetchien/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.4-test-production/3.png)
<p class="image-caption">Image 32 - HTTPS certificate on domain</p>



