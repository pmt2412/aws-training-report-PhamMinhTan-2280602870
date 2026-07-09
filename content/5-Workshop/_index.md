---
title: "Workshop"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# SaaS HR Multi-Tenant on AWS

#### Overview

A multi-tenant HR SaaS (FastAPI microservices + React) deployed on AWS as a **3-tier architecture**: CloudFront + S3 for the frontend, ECS Fargate behind an Application Load Balancer for the API, and RDS MySQL Multi-AZ for data. Async events use **Amazon SQS**, identity uses **Amazon Cognito**, monitoring uses **CloudWatch + SNS**. Built by hand in the AWS Console in `ap-southeast-1`.

Each customer company is an isolated **tenant** — data is separated per tenant.

> This workshop uses **more than 3 AWS services** (Cognito, SQS, RDS, ECS Fargate, ALB, S3, CloudFront, CloudWatch, SNS, SSM) and covers architecture design, end-to-end deployment, testing, and clean-up.

#### Content

1. [Overview & Architecture](5.1-Overview/)
2. [Prerequisite](5.2-Prerequisite/)
3. [Networking](5.3-Networking/)
4. [Data & Identity](5.4-Data-Identity/)
5. [Async Messaging (SQS)](5.5-Async-SQS/)
6. [Application Tier (ECS)](5.6-Compute-ECS/)
7. [Frontend & CDN](5.7-Frontend-CDN/)
8. [Monitoring & Alerts](5.8-Monitoring/)
9. [Testing & Validation](5.9-Testing/)
10. [Security & IAM](5.10-Security-IAM/)
11. [Clean-up](5.11-Cleanup/)


