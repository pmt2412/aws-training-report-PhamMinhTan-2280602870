---
title: "Overview & Architecture"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## 1. Idea & Objectives

#### Context & Problem
<!-- TODO: expand -->
Small and medium businesses (SMEs) need an HR system (employees, departments, payroll data) but do not want to run and maintain their own servers. A single shared SaaS must keep each company's data strictly separated.

#### Target Customer
SME companies that subscribe to the SaaS to manage HR. **Each company = one isolated tenant** (data isolation enforced per request via `tenant_id`).


#### Goals
- Deliver a multi-tenant HR SaaS on AWS with tenant data isolation.
- Highly available (Multi-AZ) and horizontally scalable application tier.
- Managed, low-ops services (no self-managed servers).

#### Success Criteria
- End-to-end login → authenticated API → data read/write works.
- Async event (tenant → HR) delivered via SQS.
- Survives an AZ / RDS failure (Multi-AZ failover).
- Monthly cost kept ~\$130 (24/7), near \$0 when torn down.

#### Fit with the program
Real-world AWS use-case using **≥3 managed services** — covers compute, data, identity, messaging, edge, and monitoring.

---

## 2. Architecture Description

#### Architecture Diagram

![Diagram](../../images/5-Workshop/5.1-Overview/architecture.png)

Three tiers inside one VPC across two Availability Zones:
- **Presentation:** CloudFront (edge) → S3 (private static, OAC).
- **Application:** ALB (public) → ECS Fargate (private) running `auth` / `tenant` / `hr`.
- **Data:** RDS MySQL Multi-AZ (private). Async via SQS; auth via app-signed RS256 JWT.

#### AWS Services Used
| Layer | Service | Role |
|:--|:--|:--|
| Edge | CloudFront | CDN + single entry (static + API) |
| Static | S3 (private + OAC) | React build artifacts |
| Identity | App-signed RS256 JWT | auth-service signs JWT (\tenant_id + role claims); tenant/hr verify via public key. Cognito pool provisioned, not wired in. |
| Load balancing | Application Load Balancer | Path-routing `/api/v1/*` to ECS |
| Compute | ECS Fargate (3 services) | auth / tenant / hr microservices |
| Registry | ECR | Container images |
| Database | RDS MySQL `db.t4g.micro` Multi-AZ | Primary + Standby, 3 logical DBs |
| Async | SQS (+ DLQ) | tenant → queue → hr |
| Secrets | SSM Parameter Store | DB password, SQS URL, JWT keys, Cognito IDs | 
| Monitoring | CloudWatch + SNS | Logs + CPU alarm → email |
| Network | VPC, IGW, 1 NAT Gateway | 6 subnets / 2 AZ |

#### Why these services (rationale)
| Service | Why chosen |
|:--|:--|
| **ECS Fargate** | Serverless containers — no EC2 to patch/scale-manage; native auto scaling. |
| **RDS Multi-AZ** | Managed HA database with synchronous standby + automatic failover + backups. |
| **SQS** | Decouples tenant→hr, absorbs spikes, survives consumer outage; DLQ for poison messages; no broker to run. |
| **App-signed RS256 JWT**  | auth-service = single issuer (private key); tenant/hr = stateless verifiers (public key). No shared secret, no external IdP. (Cognito pool provisioned but not in the flow.) |
| **CloudFront + S3** | Cheap, edge-cached static hosting; S3 stays private via OAC. |
| **ALB** | L7 path-routing to 3 services + health checks. |
| **SSM Parameter Store** | Free SecureString secrets — no hard-coded keys in images. |



