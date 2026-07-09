---
title: "Frontend & CDN"
date: 2026-07-08
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

Host bản build React riêng tư trên S3 và phục vụ mọi thứ qua CloudFront (một cổng vào cho static + API).

## Bước 11 — S3 (Static) + build frontend

1. **S3 → Create bucket** `saashr-frontend-<acct>`, **Block all public access = ON** (chỉ phục vụ qua CloudFront OAC).
![s3](../../../images/5-Workshop/5.7-Frontend-CDN/s3-1.png)
![s3](../../../images/5-Workshop/5.7-Frontend-CDN/s3-2.png)
![s3](../../../images/5-Workshop/5.7-Frontend-CDN/s3-3.png)
2. Build và upload:
```bash
cd frontend
npm run build
aws s3 sync dist/ s3://saashr-frontend-<acct> --delete
```
![s3](../../../images/5-Workshop/5.7-Frontend-CDN/s3-4.png)

![s3](../../../images/5-Workshop/5.7-Frontend-CDN/s3-5.png)

## Bước 12 — CloudFront

1. **CloudFront → Create distribution**:
![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront1.png)
![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront2.png)
   - **Origin 1** = bucket S3 qua **OAC** — behavior mặc định `/*`.
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront3.png)
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront4.png)

   - **Origin 2** = DNS của ALB — behavior `/api/v1/*`, forward hết header/cookie, tắt cache.
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront5.png)
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront6.png)
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront7.png)
   - SPA error page: 403/404 → `/index.html` (200).
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront8.png)
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront9.png)
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront10.png)
   - Create Invalidation
   ![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/cloudfront11.png)
2. **Viewer certificate:** để demo, dùng **cert mặc định của CloudFront** và truy cập qua `https://d3htvmot332c6v.cloudfront.net/`.
![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront1.png)
![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront2.png)
![cloudfront](../../../images/5-Workshop/5.7-Frontend-CDN/testcloudfront3.png)


