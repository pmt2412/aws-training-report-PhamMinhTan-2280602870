---
title: "Application Tier (ECS)"
date: 2026-07-08
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Push the images to ECR, put an ALB in front, then run the 3 services on ECS Fargate with auto scaling.

## Step 8 — ECR + push images

1. Search **ECR** → choose Private registry -> Repositories -> Create Repository: create 3 repos: `saashr-auth`, `saashr-tenant`, `saashr-hr`.

![Create repos](../../images/5-Workshop/5.6-Compute-ECS/ecr1.png)

3 repositories created successfully.
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/ecr2.png)
2. Build and push each (example for auth):
```bash
aws ecr get-login-password --region ap-southeast-1 \
  | docker login --username AWS --password-stdin <acct>.dkr.ecr.ap-southeast-1.amazonaws.com
docker build -t saashr-auth ./microservices/auth-service
docker tag saashr-auth:latest <acct>.dkr.ecr.ap-southeast-1.amazonaws.com/saashr-auth:latest
docker push <acct>.dkr.ecr.ap-southeast-1.amazonaws.com/saashr-auth:latest
```
Login aws cli
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/login-aws-ecr.png)
Push from cli (hr's push):
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/push-ecr.png)
3 ECR repos with a pushed image tag
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/push-ecr3.png)
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/push-ecr2.png)
![Create repos](../../images/5-Workshop/5.6-Compute-ECS/push-ecr1.png)
> 📎 **Attachment:** the three `Dockerfile`s (place under `5.6-Compute-ECS/files/`).

## Step 9 — Application Load Balancer
1. **Target groups** (type **IP**, one per service): 
- `tg-auth`(port 8000), `tg-tenant` (port 8001), `tg-hr`(port 8002); 
- Health check `/api/v1/{service}/health`.
- Healthy thresold = 2 (60s -> deploying faster)
![Create tg](../../images/5-Workshop/5.6-Compute-ECS/create-tg1.png)
![Create tg](../../images/5-Workshop/5.6-Compute-ECS/create-tg2.png)
![Create tg](../../images/5-Workshop/5.6-Compute-ECS/create-tg3.png)
- Continue with tg-tenant and tg-hr
![Create tg](../../images/5-Workshop/5.6-Compute-ECS/create-tg4.png)
2. Search **EC2 → Load Balancers → Create ALB** `saashr-alb`, **internet-facing**, subnets = `public-1a` + `public-1b`, SG = `sg-alb`.
- Select Application Load balancer -> create
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb1.png)
- Name: `saashr-alb`
- Choose: internet facing
- ipv4
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb2.png)
- VPC: `saashr-vpc`
- subnets = `public-1a` + `public-1b`
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb3.png)
- Listeners and routing: 
  - Choose Forward to target groups
  - protocol: HTTP
  - Port: 80
  - Target group: tg-auth 
  -> Create 
  ![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb4.png)
- ALB created successfully 
3. **Listener :80** rules → `/api/v1/auth/*`→`tg-auth`, `/api/v1/tenants/*`→`tg-tenant`, `/api/v1/hr/*`→`tg-hr`.
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb5.png)
- Then click to HTTP:80
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb6.png)
- Click Add Rule
- Conditions -> Path: /api/v1/auth/*
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb7.png)
- Target group: tg-auth
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb8.png)
- Continue with 3 rules left.
![Create alb](../../images/5-Workshop/5.6-Compute-ECS/alb9.png)


## Step 10 — ECS Fargate + Auto Scaling

1. **ECS → Create cluster** `saashr-cluster` (Fargate).
![Create cluster](../../images/5-Workshop/5.6-Compute-ECS/create-cluster.png)
2. **Task definitions** (one per service, Fargate, 0.25 vCPU / 0.5 GB):
  
   - container image = ECR repo, port = app port.

   - **Task role** (least privilege): `sqs:SendMessage/ReceiveMessage/DeleteMessage` (tenant/hr), `cognito-idp:*` minimal (auth), `ssm:GetParameters`.
   - **Execution role**: ECR pull + CloudWatch Logs + SSM read.
   - **Secrets** from SSM; log driver `awslogs` → `/ecs/saashr/{service}`.
   1.1. Create task definition for auth:
   - Name: saashr-auth
   - CPU: .25 vCPU
   - Memory: .5 GB
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-auth1.png)
   - task role: None
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy from auth repository in ECR repositories
   - port: 8000

   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-auth2.png)

  - Environment variables -> Click Add environment variable and select/paste infomation from SSM parameter store as this picture:
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-auth3.png)
  1.2. Create task definition for tenant:
  - name: saashr-tenant
  - CPU: .25 vCPU
  - Memory: .5 GB
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-tenant1.png)
  - task role: create new role
    - Role name: saashr-tenant-task-role
    - Policy -> Create new policy
    - Replace s3:GetObject with `sqs:SendMessage` and "arn:aws:s3:::[[readS3Paths]]/*" with `"arn:aws:sqs:ap-southeast-1:016461465939:saashr-events"`
    - Click Create role
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-tenant2.png)
   - task role: saashr-tenant-task-role
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy from tenant repository in ECR repositories
   - port: 8001
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-tenant3.png)
   - Environment variables -> Click Add environment variable and select/paste infomation from SSM parameter store as this picture:
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-tenant4.png)
  1.3. Create task definition for hr:
  - name: saashr-hr
  - CPU: .25 vCPU
  - Memory: .5 GB
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-hr1.png)
  - task role: create new role
    - Role name: saashr-tenant-task-role
    - Policy -> Create new policy
    - Replace "s3:GetObject" with `"sqs:ReceiveMessage","sqs:DeleteMessage","sqs:GetQueueAttributes"` and "arn:aws:s3:::[[readS3Paths]]/*" with `"arn:aws:sqs:ap-southeast-1:016461465939:saashr-events"`
    - Click Create role
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-hr2.png)
   - task role: saashr-hr-task-role
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy from tenant repository in ECR repositories
   - port: 8002
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-hr3.png)
   - Environment variables -> Click Add environment variable and select/paste infomation from SSM parameter store as this picture:
   ![td](../../images/5-Workshop/5.6-Compute-ECS/td-hr4.png)
3. **Services** (one per task def): launch type Fargate, subnets = `app-1a` + `app-1b`, SG = `sg-ecs`, **assign public IP = DISABLED**, desired count 2, attach to the matching target group.
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc1.png)
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc2.png)
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc3.png)
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc4.png)
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc5.png)
4. **Service auto scaling** (per service): target-tracking on **`ECSServiceAverageCPUUtilization` = 70%**, **min 2 / max 4**, scale-in enabled.
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-svc6.png)
- Continue with 2 service tenant and hr
- 3 services created successfully
![svc](../../images/5-Workshop/5.6-Compute-ECS/create-3svc-success.png)


{{% notice tip %}}
Auto scaling (min 2 / max 4 @ CPU 70%) is the scalability evidence for the synchronous path (rubric 4.2). Screenshot the policy on all three services.
{{% /notice %}}
