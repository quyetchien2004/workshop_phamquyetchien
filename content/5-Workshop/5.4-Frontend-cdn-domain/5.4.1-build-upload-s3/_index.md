---
title: "Build frontend and upload to Amazon S3"
date: 2026-07-06
weight: 541
chapter: false
pre: " <b> 5.4.1 </b> "
---

## Objective

This step builds the React/Vite frontend and uploads the files inside `frontend/dist` to Amazon S3. S3 stores static files only, while CloudFront serves HTTPS traffic and caches the content.

## 1. Configure frontend production environment

Inside the `frontend` folder, create or update `.env.production`:

```env
VITE_API_URL=https://www.viet70speed.xyz/api
VITE_SOCKET_URL=https://www.viet70speed.xyz
```

If the custom domain is not ready yet, use the CloudFront domain:

```env
VITE_API_URL=https://d2r7fm91wsef8d.cloudfront.net/api
VITE_SOCKET_URL=https://d2r7fm91wsef8d.cloudfront.net
```

Do not use `localhost` in production builds.

![Image 20 - Frontend production env file](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/1.png)
<p class="image-caption">Image 20 - Frontend production env file</p>

## 2. Build frontend

Run:

```bash
npm run build --workspace frontend
```

Or from inside the `frontend` folder:

```bash
npm run build
```

After a successful build, `frontend/dist` contains `index.html` and the `assets/` folder.

![Image 21 - Frontend build success](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/2.png)
<p class="image-caption">Image 21 - Frontend build success</p>

## 3. Upload to S3

Open AWS Console > S3:

1. Select bucket `cct-hotels-frontend-bucket`.
2. Click **Upload**.
3. Select all files and folders **inside** `frontend/dist`; do not upload the `dist` folder itself.
4. Click **Upload**.

![Image 22 - Upload frontend dist to S3](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/3.png)
<p class="image-caption">Image 22 - Upload frontend dist to S3</p>

## 4. Create CloudFront invalidation

After each new frontend upload, clear CloudFront cache:

1. Open CloudFront > Distributions.
2. Select `cct-hotels-frontend-cdn`.
3. Open **Invalidations**.
4. Click **Create invalidation**.
5. Enter object path: `/*`.

![Image 23 - CloudFront invalidation](/images/5-Workshop/5.4-Frontend-cdn-domain/5.4.1-build-upload-s3/4.png)
<p class="image-caption">Image 23 - CloudFront invalidation</p>



