---
title: "Tầng ứng dụng (ECS)"
date: 2026-07-08
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Push image lên ECR, đặt ALB phía trước, rồi chạy 3 service trên ECS Fargate với auto scaling.

## Bước 8 — ECR + push image

1. Tìm **ECR** → chọn Private registry -> Repositories -> Create Repository: tạo 3 repo: `saashr-auth`, `saashr-tenant`, `saashr-hr`.

![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/ecr1.png)

Tạo thành công 3 repository.
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/ecr2.png)
2. Build và push từng cái (ví dụ auth):
```bash
aws ecr get-login-password --region ap-southeast-1 \
  | docker login --username AWS --password-stdin <acct>.dkr.ecr.ap-southeast-1.amazonaws.com
docker build -t saashr-auth ./microservices/auth-service
docker tag saashr-auth:latest <acct>.dkr.ecr.ap-southeast-1.amazonaws.com/saashr-auth:latest
docker push <acct>.dkr.ecr.ap-southeast-1.amazonaws.com/saashr-auth:latest
```
Đăng nhập aws cli
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/login-aws-ecr.png)
Push từ cli (push của hr):
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/push-ecr.png)
3 repo ECR đã có tag image push lên
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/push-ecr3.png)
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/push-ecr2.png)
![Create repos](../../../images/5-Workshop/5.6-Compute-ECS/push-ecr1.png)
> 📎 **Đính kèm:** ba file `Dockerfile` (đặt vào `5.6-Compute-ECS/files/`).

## Bước 9 — Application Load Balancer
1. **Target group** (type **IP**, mỗi service một cái): 
- `tg-auth`(port 8000), `tg-tenant` (port 8001), `tg-hr`(port 8002); 
- Health check `/api/v1/{service}/health`.
- Healthy threshold = 2 (60s -> deploy nhanh hơn)
![Create tg](../../../images/5-Workshop/5.6-Compute-ECS/create-tg1.png)
![Create tg](../../../images/5-Workshop/5.6-Compute-ECS/create-tg2.png)
![Create tg](../../../images/5-Workshop/5.6-Compute-ECS/create-tg3.png)
- Tiếp tục với tg-tenant và tg-hr
![Create tg](../../../images/5-Workshop/5.6-Compute-ECS/create-tg4.png)
2. Tìm **EC2 → Load Balancers → Create ALB** `saashr-alb`, **internet-facing**, subnet = `public-1a` + `public-1b`, SG = `sg-alb`.
- Chọn Application Load balancer -> create
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb1.png)
- Name: `saashr-alb`
- Chọn: internet facing
- ipv4
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb2.png)
- VPC: `saashr-vpc`
- subnets = `public-1a` + `public-1b`
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb3.png)
- Listeners and routing: 
  - Chọn Forward to target groups
  - protocol: HTTP
  - Port: 80
  - Target group: tg-auth 
  -> Create 
  ![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb4.png)
- Tạo ALB thành công 
3. **Listener :80** rule → `/api/v1/auth/*`→`tg-auth`, `/api/v1/tenants/*`→`tg-tenant`, `/api/v1/hr/*`→`tg-hr`.
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb5.png)
- Rồi bấm vào HTTP:80
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb6.png)
- Bấm Add Rule
- Conditions -> Path: /api/v1/auth/*
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb7.png)
- Target group: tg-auth
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb8.png)
- Tiếp tục với 3 rule còn lại.
![Create alb](../../../images/5-Workshop/5.6-Compute-ECS/alb9.png)


## Bước 10 — ECS Fargate + Auto Scaling

1. **ECS → Create cluster** `saashr-cluster` (Fargate).
![Create cluster](../../../images/5-Workshop/5.6-Compute-ECS/create-cluster.png)
2. **Task definition** (mỗi service một cái, Fargate, 0.25 vCPU / 0.5 GB):
  
   - image container = repo ECR, port = cổng app.

   - **Task role** (quyền tối thiểu): `sqs:SendMessage/ReceiveMessage/DeleteMessage` (tenant/hr), `cognito-idp:*` tối thiểu (auth), `ssm:GetParameters`.
   - **Execution role**: pull ECR + CloudWatch Logs + đọc SSM.
   - **Secret** lấy từ SSM; log driver `awslogs` → `/ecs/saashr/{service}`.
   1.1. Tạo task definition cho auth:
   - Name: saashr-auth
   - CPU: .25 vCPU
   - Memory: .5 GB
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-auth1.png)
   - task role: None
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy từ repository auth trong ECR repositories
   - port: 8000

   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-auth2.png)

  - Environment variables -> Bấm Add environment variable và chọn/dán thông tin từ SSM parameter store như hình:
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-auth3.png)
  1.2. Tạo task definition cho tenant:
  - name: saashr-tenant
  - CPU: .25 vCPU
  - Memory: .5 GB
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-tenant1.png)
  - task role: create new role
    - Role name: saashr-tenant-task-role
    - Policy -> Create new policy
    - Thay s3:GetObject bằng `sqs:SendMessage` và "arn:aws:s3:::[[readS3Paths]]/*" bằng `"arn:aws:sqs:ap-southeast-1:016461465939:saashr-events"`
    - Bấm Create role
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-tenant2.png)
   - task role: saashr-tenant-task-role
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy từ repository tenant trong ECR repositories
   - port: 8001
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-tenant3.png)
   - Environment variables -> Bấm Add environment variable và chọn/dán thông tin từ SSM parameter store như hình:
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-tenant4.png)
  1.3. Tạo task definition cho hr:
  - name: saashr-hr
  - CPU: .25 vCPU
  - Memory: .5 GB
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-hr1.png)
  - task role: create new role
    - Role name: saashr-tenant-task-role
    - Policy -> Create new policy
    - Thay "s3:GetObject" bằng `"sqs:ReceiveMessage","sqs:DeleteMessage","sqs:GetQueueAttributes"` và "arn:aws:s3:::[[readS3Paths]]/*" bằng `"arn:aws:sqs:ap-southeast-1:016461465939:saashr-events"`
    - Bấm Create role
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-hr2.png)
   - task role: saashr-hr-task-role
   - task execution role: ecsTaskExecutionRole
   - Container name: saashr-auth
   - Image URI: copy từ repository tenant trong ECR repositories
   - port: 8002
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-hr3.png)
   - Environment variables -> Bấm Add environment variable và chọn/dán thông tin từ SSM parameter store như hình:
   ![td](../../../images/5-Workshop/5.6-Compute-ECS/td-hr4.png)
3. **Service** (mỗi task def một cái): launch type Fargate, subnet = `app-1a` + `app-1b`, SG = `sg-ecs`, **assign public IP = DISABLED**, desired count 2, gắn vào target group tương ứng.
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc1.png)
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc2.png)
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc3.png)
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc4.png)
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc5.png)
4. **Service auto scaling** (mỗi service): target-tracking theo **`ECSServiceAverageCPUUtilization` = 70%**, **min 2 / max 4**, cho phép scale-in.
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-svc6.png)
- Tiếp tục với 2 service tenant và hr
- Tạo thành công 3 service
![svc](../../../images/5-Workshop/5.6-Compute-ECS/create-3svc-success.png)


{{% notice tip %}}
Auto scaling (min 2 / max 4 @ CPU 70%) là bằng chứng co giãn cho luồng đồng bộ (rubric 4.2). Chụp policy trên cả ba service.
{{% /notice %}}


