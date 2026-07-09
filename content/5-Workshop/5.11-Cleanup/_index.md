---
title: "Clean-up"
date: 2026-07-08
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

Delete resources in reverse dependency order to avoid ongoing charges. The **NAT Gateway, RDS, and ALB bill by the hour** — remove these first if you only want to pause.

## Teardown order

1. **CloudFront** — disable, then delete the distribution.
2. **S3** — empty, then delete `saashr-frontend-<acct>`.
3. **ALB** — delete the load balancer + the 3 target groups.
4. **ECS** — set services to desired 0, delete the services, then the cluster.
5. **NAT Gateway** — delete (stops the biggest hourly charge).
6. **RDS** — delete the database instance (skip or take a final snapshot).
7. **Elastic IP** — release the NAT's EIP.
8. **VPC** — delete (removes subnets, route tables, IGW, security groups).

## Then remove the standalone services:
9. **SNS** topic + **CloudWatch** alarms + log groups.
10. **ECR** repositories (delete images first).
11. **Cognito** user pool, **SSM** parameters.



{{% notice warning %}}
Double-check the **Billing → Budgets** page a day later to confirm charges have stopped. Leftover NAT Gateways and RDS instances are the usual surprise bills.
{{% /notice %}}
