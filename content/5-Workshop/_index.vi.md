---
title: "Workshop"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# SaaS HR Multi-Tenant trên AWS

#### Tổng quan

Một hệ SaaS quản lý nhân sự đa khách hàng (microservices FastAPI + React) triển khai trên AWS theo **kiến trúc 3 tầng**: CloudFront + S3 cho frontend, ECS Fargate sau Application Load Balancer cho API, và RDS MySQL Multi-AZ cho dữ liệu. Sự kiện bất đồng bộ dùng **Amazon SQS**, xác thực dùng **Amazon Cognito**, giám sát dùng **CloudWatch + SNS**. Xây thủ công trên AWS Console tại vùng `ap-southeast-1`.

Mỗi công ty khách hàng là một **tenant** riêng biệt — dữ liệu được cách ly theo từng tenant.

> Workshop này dùng **hơn 3 dịch vụ AWS** (Cognito, SQS, RDS, ECS Fargate, ALB, S3, CloudFront, CloudWatch, SNS, SSM) và bao gồm: thiết kế kiến trúc, triển khai end-to-end, kiểm thử và dọn dẹp.

#### Nội dung

1. [Tổng quan & Kiến trúc](5.1-Overview/)
2. [Chuẩn bị](5.2-Prerequisite/)
3. [Mạng](5.3-Networking/)
4. [Dữ liệu & Danh tính](5.4-Data-Identity/)
5. [Hàng đợi bất đồng bộ (SQS)](5.5-Async-SQS/)
6. [Tầng ứng dụng (ECS)](5.6-Compute-ECS/)
7. [Frontend & CDN](5.7-Frontend-CDN/)
8. [Giám sát & Cảnh báo](5.8-Monitoring/)
9. [Kiểm thử & Xác thực](5.9-Testing/)
10. [Bảo mật & IAM](5.10-Security-IAM/)
11. [Dọn dẹp](5.11-Cleanup/)
