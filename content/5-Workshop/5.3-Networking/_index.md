---
title: "Networking"
date: 2026-07-08
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Build the network foundation first: one VPC, 6 subnets across 2 AZ, an Internet Gateway, one NAT Gateway, and the security groups.

## Step 1 — VPC + 6 Subnets + IGW + NAT

1. **VPC → Create VPC** — name `saashr-vpc`, CIDR `10.0.0.0/16`.


![Create VPC](../../images/5-Workshop/5.3-Networking/create-vpc1.png)

- VPC -> Created VPC

![Created VPC](../../images/5-Workshop/5.3-Networking/create-vpc2.png)
2. **Subnets** — create 6:

| Subnet name| AZ | CIDR | Purpose |
|:--|:--|:--|:--|
| `public-1a` | 1a | `10.0.1.0/24` | ALB, NAT |
| `public-1b` | 1b | `10.0.2.0/24` | ALB |
| `app-1a` | 1a | `10.0.11.0/24` | ECS Fargate |
| `app-1b` | 1b | `10.0.12.0/24` | ECS Fargate |
| `data-1a` | 1a | `10.0.21.0/24` | RDS Primary |
| `data-1b` | 1b | `10.0.22.0/24` | RDS Standby |

- VPC -> Subnets -> Create Subnet
![Create subnet](../../images/5-Workshop/5.3-Networking/create-subnet1.png)

- 6 subnets create successfully
![Create subnet](../../images/5-Workshop/5.3-Networking/create-subnet2.png)

3. **Internet Gateway** — create `saashr-igw`, **attach to VPC**.
- VPC -> Internet gateway -> create internet gateway
![Create internet gateway](../../images/5-Workshop/5.3-Networking/create-igw.png)

- Actions -> Attach to VPC
4. **NAT Gateway** — create **1** in `public-1a`, allocate an Elastic IP.
- Choose VPC -> Elastic IP addresses -> Allocate Elastic IP Address
![Create elastic ip](../../images/5-Workshop/5.3-Networking/create-elastic-ip.png)

- Click Allocate

- Click VPC -> NAT gateway -> Create NAT gateway
![Create nat gateway](../../images/5-Workshop/5.3-Networking/create-nat.png)
- Creation complete
![Create nat gateway](../../images/5-Workshop/5.3-Networking/create-nat2.png)
5. **Route tables:**
   - `rt-public` → `0.0.0.0/0` → **IGW** → associate `public-1a`, `public-1b`.
   - `rt-app` → `0.0.0.0/0` → **NAT Gateway** → associate `app-1a`, `app-1b`.
   - `rt-data` → **no internet route** (local only) → associate `data-1a`, `data-1b`.

- Click VPC -> route table -> create route table
![Create route table](../../images/5-Workshop/5.3-Networking/create-route-table1.png)

![Create route table](../../images/5-Workshop/5.3-Networking/create-route-table2.png)

- Click Actions -> Edit subnet associations
![Create route table](../../images/5-Workshop/5.3-Networking/associate.png)

- Click Edit routes
![Create route table](../../images/5-Workshop/5.3-Networking/edit-route.png)
- 3 route tables created successfully

![Create route table](../../images/5-Workshop/5.3-Networking/create-route-table.png)


## Step 2 — Security Groups (zero-trust)

| SG | Ingress | Egress |
|:--|:--|:--|
| **`sg-alb`** | 80/443 from `0.0.0.0/0` | all |
| **`sg-ecs`** | app port (8000–8002) **from `sg-alb` only** | all (NAT → Cognito/SQS/ECR/CloudWatch) |
| **`sg-rds`** | 3306 **from `sg-ecs` only** | none |

The 3 security groups 

- ALB security group inbound and outbound
![Create security group](../../images/5-Workshop/5.3-Networking/alb-sg-inbound.png)
![Create security group](../../images/5-Workshop/5.3-Networking/alb-sg-outbound.png)

- ECS security group inbound and outbound
![Create security group](../../images/5-Workshop/5.3-Networking/ecs-sg-inbound.png)
![Create security group](../../images/5-Workshop/5.3-Networking/ecs-sg-outbound.png)

- RDS security group inbound and outbound
![Create security group](../../images/5-Workshop/5.3-Networking/rds-sg-inbound.png)
![Create security group](../../images/5-Workshop/5.3-Networking/rds-sg-outbound.png)

{{% notice note %}}
Chained SGs (alb → ecs → rds) mean the database can never be reached from the internet — only from the ECS tasks. See [Security & IAM](../5.10-Security-IAM/).
{{% /notice %}}
