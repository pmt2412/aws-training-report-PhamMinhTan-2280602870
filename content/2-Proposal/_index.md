---
title: "Proposal"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# SaaS HR Multi-Tenant Microservices
## A Unified AWS Cloud Solution for Enterprise-Scale Multi-Tenant HR Management

### 1. Executive Summary
SaaS HR Multi-Tenant Microservices is an enterprise-scale multi-tenant Human Resource management (SaaS) platform designed to enhance HR administration efficiency and optimize operational costs for organizations. The system supports up to thousands of independent businesses (tenants) operating on the same platform thanks to a strict logical data separation model (Pool Model). The solution leverages AWS Cloud services (ECS Fargate, RDS MySQL, SQS, Cognito) combined with asymmetric authentication via Nginx API Gateway to provide a secure, safe, and cost-effective real-time experience for system health monitoring, employee management, timekeeping, and leave management.

### 2. Problem Statement
*Current Problem*
Traditional HR management systems are often deployed individually for each company, leading to wasted server resources and high maintenance effort. For multi-tenant solutions, the primary challenge is ensuring absolute data isolation between client enterprises (preventing cross-tenant data leakage) while maintaining high performance during traffic spikes, and effectively controlling infrastructure costs.

*The Solution*
The project develops a SaaS HR Multi-Tenant system based on a Microservices architecture decomposed into 3 core services: `auth-service` (authentication), `tenant-service` (tenant management), and `hr-service` (HR operations). The system implements logical data isolation (Pool Model) across 3 independent RDS MySQL databases (`auth_db`, `tenant_db`, `hr_db`). Using Amazon Cognito as a centralized identity provider integrated with RS256 JWT signing, it enables services to perform stateless token self-verification extremely fast. The ReactJS (Vite) frontend is hosted on S3 and distributed globally via CloudFront CDN. Asynchronous events between services are decoupled through Amazon SQS to prevent bottlenecking.

*Benefits and Return on Investment (ROI)*
- **Infrastructure Cost Optimization**: By combining the 3 microservices databases into a single RDS MySQL Multi-AZ instance and leveraging serverless ECS Fargate, the estimated AWS infrastructure operational cost is optimized to around $115 - $155 USD/month (running 24/7). All infrastructure can be torn down (destroyed) when not in use to reduce costs to near $0 USD.
- **High Availability & Auto Backup**: Multi-AZ deployment minimizes downtime, provides auto-recovery, and supports automatic 7-day data backup retention.
- **Reduced Deployment Time**: New tenant clients can register and use the system immediately via a dedicated subdomain without any physical server installation or configuration steps. The estimated break-even period for the system is 3–6 months, driven by optimized employee productivity and reduced IT operational costs.

### 3. Solution Architecture
The platform adopts a 3-tier architecture combined with Microservices on AWS to manage and operate the application. All compute resources are deployed within a VPC spanning 2 Availability Zones (AZs) to enhance fault tolerance.

![AWS Architecture Diagram - SaaS HR Multi-Tenant](/images/2-Proposal/architecture.png)

*AWS Services Used*
- **Amazon CloudFront**: CDN distributing the static frontend and acting as the single point of entry for the entire system.
- **Amazon S3**: Stores the statically built source code of the ReactJS application (securely accessed via OAC).
- **Application Load Balancer (ALB)**: Receives traffic from CloudFront to route `/api/v1/*` requests into the ECS Fargate cluster.
- **Amazon ECS Fargate**: Runs stateless microservices containers (`auth`, `tenant`, `hr`) across both AZs.
- **Amazon RDS MySQL**: Multi-AZ Database Management System (Primary & Standby) running in private subnets, completely isolated from the internet.
- **Amazon SQS**: Manages message queues for asynchronous communication between `tenant-service` and `hr-service`.
- **Amazon Cognito**: User Pools managing user accounts and issuing JWTs containing tenant ID claims.
- **AWS NAT Gateway, Route 53, ACM, CloudWatch, SNS, SSM Parameter Store**.

*Component Design*
- **Client Interface (Frontend)**: ReactJS (Vite) handles client-side routing, automatically detects the tenant via subdomain, and stores authentication tokens.
- **API Gateway & Load Balancing**: ALB dynamically routes to the corresponding target groups in ECS. Security Groups are configured following a Zero-Trust model, allowing only the ALB to connect to ECS Fargate, and ECS Fargate to connect to RDS MySQL.
- **Service & Business Logic Layer**: FastAPI microservices process logic independently. JWT tokens are verified statelessly at each service using the public key retrieved from Cognito, eliminating the need to call the authentication service.
- **Database & Isolation Layer**: Data is stored centrally in RDS MySQL. The `employees` and `departments` tables enforce logical isolation through a mandatory `tenant_id` field, optimized with indexes to accelerate queries and guarantee data isolation.

### 4. Technical Implementation
*Implementation Phases*
The project is implemented in parallel between code development and Cloud infrastructure setup through 4 key phases:
1. *Network Infrastructure & Security Design*: Establish VPC, partitioning 6 isolated subnets for 3 tiers (Public, App Private, Data Private). Configure Route Tables and Security Groups.
2. *Local Source Code Development*: Program the 3 FastAPI microservices, build the ReactJS interface, configure Nginx as a local API Gateway, and integrate the RS256 JWT mechanism.
3. *Integration & Data Security Verification*: Perform testing of data isolation between tenants, and integrate the asynchronous message queuing system via Redis/SQS.
4. *Cloud Deployment & Optimization*: Package Docker containers (multi-stage build), push to ECR, configure and launch services on AWS ECS Fargate, ALB, RDS Multi-AZ, and Cognito, and configure the CloudWatch Logs monitoring system.

*Technical Requirements*
- **Local Development Workstation**: Docker Desktop installed to run MySQL, Nginx Gateway, and 3 microservices in sync. Python 3.10+ for service APIs, and Node.js v18+ for the React frontend.
- **AWS Cloud Platform**: Requires an active AWS account with full permissions to provision VPC, ECS Fargate, RDS MySQL, Cognito User Pools, SQS, CloudFront, and S3. Proficient knowledge of optimized Docker packaging and asymmetric key verification mechanisms (RS256) is required.

### 5. Timeline & Milestones
- **Sprint 1: Infrastructure Initialization & API Gateway Setup (Week 1)**: Project directory structuring, running sample database migrations (Alembic). Configure local Nginx gateway and React Vite scaffolding for login/dashboard.
- **Sprint 2: Core Microservices & DB Association (Week 2)**: Build complete `auth-service` (RS256 authentication), `tenant-service` (subscription package management), and `hr-service` with core data schemas supporting logical isolation via `tenant_id`.
- **Sprint 3: System Integration & Subdomain Routing (Week 3)**: Connect ReactJS frontend with API Gateway, configure Axios interceptors to auto-attach tokens, handle user authorization, and design intuitive HR management interfaces.
- **Sprint 4: Container Packaging, Security & Cloud Deployment (Week 4)**: Light production Docker packaging (multi-stage), security scanning (Trivy), provisioning AWS infrastructure, and conducting final system verification tests with a single command.

### 6. Budget Estimation
Operational cost estimation on AWS (Singapore Region, running 24/7):

| AWS Service | Proposed Configuration | Estimated / Month |
| :--- | :--- | :---: |
| **NAT Gateway** | 1 shared NAT Gateway in the public subnet | **$43 – $50** |
| **RDS MySQL Multi-AZ** | `db.t4g.micro` instance x2, 20 GB gp3 storage & backups | **$30 – $38** |
| **Application Load Balancer** | 1 ALB for load balancing + ~1 LCU | **$22 – $26** |
| **ECS Fargate** | 4–6 tasks running services (0.25 vCPU / 0.5 GB) | **$12 – $18** |
| **Data Transfer / CloudWatch** | Logs retained 7 days, NAT data transfer, inter-AZ | **$5 – $12** |
| **Route 53 + CloudFront + S3 + ECR** | Domain, CDN, React static hosting, Container registry | **$3 – $8** |
| **Cognito / SQS / SNS / ACM** | Covered under Free Tier limits | **$0** |
| **Total / Month** | | **≈ $115 – $155 (~$130)** |

> [!NOTE]
> To optimize study and testing costs, all resources can be created and cleaned up (destroyed) quickly using CDK/Terraform scripts when not running a demo.

### 7. Risk Assessment
*Risk Matrix*
- **Cross-tenant Data Leakage**: Impact: Critical; Probability: Low.
- **API Gateway or Microservice Overload**: Impact: High; Probability: Medium.
- **AWS Budget Overrun**: Impact: Medium; Probability: Medium.

*Mitigation Strategies*
- **Data Leak Prevention**: Establish verification and auto-injection of `tenant_id` conditions (extracted from verified JWT token claims) into all database query statements using FastAPI Dependency Injection.
- **Overload Prevention**: Configure Auto Scaling for ECS Fargate based on CPU/Memory usage, and decouple intensive processing workloads asynchronously using SQS queues.
- **Cost Control**: Activate AWS Budgets to alert via email (SNS) when projected or actual costs exceed $100 USD or $130 USD.

*Contingency Plans*
- Use automated cleanup tools/scripts for unused AWS resources (orphaned volumes, unused NAT gateways, stale Elastic IPs).
- Store RDS backup copies in compressed, secure, and isolated locations to recover from disaster scenarios.

### 8. Expected Outcomes
- **Technical Improvements**: A smoothly running multi-tenant system with absolute data isolation between clients, automated subdomain routing, and enterprise-grade secure authentication (RS256 JWT).
- **Long-term Value**: Establish high-quality reference design documentation and microservices architecture source code that is easily reusable for future SaaS projects.