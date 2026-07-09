---
title: "Frontend & CDN"
date: 2026-07-08
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Host the React build privately on S3 and serve everything through CloudFront (single entry for static + API).

## Step 11 — S3 (Static) + frontend build

1. **S3 → Create bucket** `saashr-frontend-<acct>`, **Block all public access = ON** (served only via CloudFront OAC).
![s3](../../images/5-Workshop/5.7-Frontend-CDN/s3-1.png)
![s3](../../images/5-Workshop/5.7-Frontend-CDN/s3-2.png)
![s3](../../images/5-Workshop/5.7-Frontend-CDN/s3-3.png)
2. Build and upload:
```bash
cd frontend
npm run build
aws s3 sync dist/ s3://saashr-frontend-<acct> --delete
```
![s3](../../images/5-Workshop/5.7-Frontend-CDN/s3-4.png)

![s3](../../images/5-Workshop/5.7-Frontend-CDN/s3-5.png)

## Step 12 — CloudFront

1. **CloudFront → Create distribution**:
![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront1.png)
![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront2.png)
   - **Origin 1** = the S3 bucket via **OAC** — default behavior `/*`.
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront3.png)
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront4.png)

   - **Origin 2** = the ALB DNS — behavior `/api/v1/*`, forward all headers/cookies, caching disabled.
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront5.png)
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront6.png)
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront7.png)
   - SPA error pages: 403/404 → `/index.html` (200).
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront8.png)
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront9.png)
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront10.png)
   - Create Invalidation
   ![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/cloudfront11.png)
2. **Viewer certificate:** for a demo, use the **default CloudFront certificate** and access via `https://d3htvmot332c6v.cloudfront.net/`.
![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront1.png)
![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront2.png)
![cloudfront](../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront3.png)



