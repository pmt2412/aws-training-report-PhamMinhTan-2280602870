---
title: "Security & IAM"
date: 2026-07-08
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

How the design applies least privilege, keeps resources private, and avoids hard-coded credentials.

## Principle of Least Privilege

Each ECS service assumes its own **task role** with only the actions it needs:

| Service | Allowed actions |
|:--|:--|
| `tenant` | `sqs:SendMessage` on `saashr-events` |
| `hr` | `sqs:ReceiveMessage`, `sqs:DeleteMessage` on `saashr-events` |
| `auth` | minimal `cognito-idp` (InitiateAuth), `ssm:GetParameters` |
| all | `ssm:GetParameters` (own params), CloudWatch Logs write |

Example — the `tenant` task role (scoped to one queue, one action):
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "sqs:SendMessage",
    "Resource": "arn:aws:sqs:ap-southeast-1:<acct>:saashr-events"
  }]
}
```

The task role with its inline least-privilege policy → 
![Business operations](../../images/5-Workshop/5.10-Security-IAM/taskrole.png)

## No hard-coded credentials
- boto3 uses the **ECS task-role credentials** at runtime — **no static access keys** in code or images.
- Secrets (DB password, SQS URL, Cognito IDs) come from **SSM Parameter Store** at task start, not from the repo.

## Private by default
- **ECS** and **RDS** run in **private subnets** with **no public IP**.
- **S3** blocks all public access; served only via **CloudFront OAC**.
- **RDS** data subnets have **no route to the internet**.

## Zero-trust security groups
`sg-alb` → `sg-ecs` → `sg-rds` are chained so each tier only accepts traffic from the tier in front (see [Networking](../5.3-Networking/)). The database is unreachable from the internet.

> 📷 **[Screenshot]** S3 "Block all public access = On" + RDS "Publicly accessible = No" → `/images/5-Workshop/5.10-Security-IAM/private-resources.png`
![Business operations](../../images/5-Workshop/5.10-Security-IAM/s3.png)
![Business operations](../../images/5-Workshop/5.10-Security-IAM/rds.png)
