---
title: "Test backend and database"
date: 2026-07-06
weight: 532
chapter: false
pre: " <b> 5.3.2 </b> "
---

After deploying the backend, test the system from infrastructure level to application level.

## 1. Check Elastic Beanstalk status

The environment `cct-hotels-backend-env-v2` should show:

- Health: `Ok`
- Platform: Node.js on Amazon Linux 2023
- App port: `8080`
- Health check path: `/api/health`

![Image 13 - Elastic Beanstalk Health OK](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/1.png)
<p class="image-caption">Image 13 - Elastic Beanstalk Health OK</p>

## 2. Test API directly through Elastic Beanstalk

Open:

```text
http://<elastic-beanstalk-domain>/api/health
```

Expected result:

```json
{
  "status": "ok",
  "message": "Backend healthy"
}
```

![Image 14 - Backend health through Elastic Beanstalk](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/2.png)
<p class="image-caption">Image 14 - Backend health through Elastic Beanstalk</p>

## 3. Test API through CloudFront

After adding the `/api/*` CloudFront behavior, test:

```text
https://d2r7fm91wsef8d.cloudfront.net/api/health
```

Or with the custom domain:

```text
https://www.viet70speed.xyz/api/health
```

If the API returns a healthy JSON response, CloudFront is forwarding API traffic correctly.

![Image 15 - Backend health through CloudFront](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/3.png)
<p class="image-caption">Image 15 - Backend health through CloudFront</p>

## 4. Test DocumentDB connectivity

Use application actions that read/write data:

- Register a new account.
- Log in.
- Browse rooms.
- Create a test booking.
- View the booking from the admin page.

If these actions work, the backend can connect to DocumentDB successfully.

![Image 16 - Registration or login success](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/4.png)
<p class="image-caption">Image 16 - Registration or login success</p>

![Image 17 - Booking saved in the system](/workshop_phamquyetchien/images/5-Workshop/5.3-Network-backend-database/5.3.2-test-backend-database/5.png)
<p class="image-caption">Image 17 - Booking saved in the system</p>

## 5. Common issues

| Error | Common cause | Fix |
|---|---|---|
| `Not allowed by CORS` | `CLIENT_URL` does not include the frontend domain | Add CloudFront/custom domain to `CLIENT_URL`, then apply EB config |
| CloudFront `/api/health` returns 504 | Backend origin uses HTTPS while EB is HTTP | Set CloudFront backend origin protocol to HTTP only or proper match viewer setting |
| EB health is Severe | Wrong port, wrong health path, or app crash | Check `PORT=8080`, `/api/health`, and EB logs |
| DocumentDB connection fails | Wrong SG, URI, subnet, or CA bundle | Check `cct-docdb-sg`, `MONGO_URI`, and `global-bundle.pem` |


