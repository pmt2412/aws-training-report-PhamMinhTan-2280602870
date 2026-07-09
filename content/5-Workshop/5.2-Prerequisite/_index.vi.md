---
title: "Chuẩn bị"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Chuẩn bị

| Yêu cầu | Chi tiết |
|:--|:--|
| **Tài khoản AWS** | Một tài khoản dùng chung. Khóa root, bật MFA. |
| **Region** | `ap-southeast-1` (Singapore). `us-east-1` chỉ dùng cho cert ACM của CloudFront (nếu có domain riêng). |
| **Truy cập nhóm** | **IAM Identity Center** — mỗi thành viên một SSO user (không dùng chung root, không static key). |
| **AWS CLI** | Cấu hình qua IAM Identity Center (để push ECR, sync S3). |
| **Docker** | Để build và push 3 image service. |

---

## 1. Truy cập nhóm bằng IAM Identity Center

Cả nhóm làm việc trên một tài khoản AWS dùng chung. Thay vì chia sẻ root hay phát access key, mỗi thành viên có **SSO user riêng** qua **IAM Identity Center**, gom nhóm và cấp quyền bằng permission set.

> ℹ️ Các SSO user này dành cho **người xây stack** (console + CLI). Chúng tách biệt với **IAM role runtime của ứng dụng** (vẫn giữ quyền tối thiểu) — xem [Bảo mật & IAM](../5.10-Security-IAM/).

#### 1.1. Mở IAM Identity Center
Tìm và mở **IAM Identity Center** trong console, rồi bật (enable) nó.

![search IIC](../../../images/5-Workshop/5.2-Prerequisite/search-iic.png)

#### 1.2. Tạo user và group
1. Menu bên trái → **Users → Add user**. Điền các trường bắt buộc (Username, Email, First name, Last name) → **Next** để hoàn tất.
![add user](../../../images/5-Workshop/5.2-Prerequisite/add-user.png)
![add user](../../../images/5-Workshop/5.2-Prerequisite/add-user1.png)
2. **Groups → Create group** — đặt tên group và thêm các user vừa tạo (dễ quản lý).
![create group](../../../images/5-Workshop/5.2-Prerequisite/create-group.png)


#### 1.3. Tạo permission set
1. **Permission sets → Create permission set**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/create-permission-set.png)
2. Permission set type = **Predefined permission set**.
3. Policy = **AdministratorAccess**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/create-permission-set1.png)

4. Đặt **tên** và **Session duration**, rồi **Create**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/create-permission-set2.png)
![create ps](../../../images/5-Workshop/5.2-Prerequisite/create-permission-set3.png)
> 📷 **[Ảnh]** Predefined = AdministratorAccess → `/images/5-Workshop/5.2-Prerequisite/iic-permset.png`

#### 1.4. Cấp quyền truy cập vào AWS account
1. Menu bên trái → dưới **Multi-account permissions**, chọn **AWS accounts**.
2. Tích tài khoản AWS cần cấp quyền → **Assign users or groups**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-account.png)
3. Chọn user/group đã tạo → **Next**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-account2.png)

4. Chọn permission set → **Next**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-account3.png)

5. Kiểm tra và **Submit**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-account4.png)
![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-account5.png)


#### 1.5. Đăng nhập qua AWS access portal
Trở lại **Dashboard** của IAM Identity Center, mở **URL AWS access portal** và đăng nhập bằng SSO user.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/open-link1.png)
![create ps](../../../images/5-Workshop/5.2-Prerequisite/sign-in.png)


---

## 2. Cấu hình AWS CLI (qua IAM Identity Center)

Kết nối CLI với access portal để dùng credential SSO ngắn hạn (không static key) — cần cho push ECR và sync S3.

```bash
aws configure sso
# SSO start URL:   https://<your-portal>.awsapps.com/start
# SSO Region:      ap-southeast-1
# (trình duyệt mở → approve) → chọn account + permission set
# CLI default Region: ap-southeast-1
# CLI profile name:   saashr

# bắt đầu / làm mới session:
aws sso login --profile saashr
aws sts get-caller-identity --profile saashr
```

![create ps](../../../images/5-Workshop/5.2-Prerequisite/aws-conf.png)

> ⚠️ **Kiểm chứng:** xác nhận các bước CLI này khớp với những gì bạn đã chạy thực tế. Nếu bạn dùng tên profile hoặc cách khác, báo tôi để chỉnh.

---

## 3. Docker
Cài Docker Desktop (hoặc Docker Engine) để build và push image `auth` / `tenant` / `hr` lên ECR ở [Tầng ứng dụng](../5.6-Compute-ECS/).

---

## Bước 0 — Lưới an toàn chi phí (AWS Budgets)
Thiết lập budget với cảnh báo email để phát hiện vượt chi phí sớm (miễn phí).

1. Tìm và mở **Billing and Cost Management → Budgets**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget1.png)
2. Bấm **Create budget**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget2.png)


3. **Budget setup = Customize (advanced)**; **Budget type = Cost budget – Recommended** → **Next**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget3.png)
4. Cấu hình budget:
   - **Budget name:** `SaaS-HR_Multi-tenant-Budget`
   - **Period:** Custom — Start `2026/06/27`, End `2026/07/27`
   - **Budgeted amount:** `130.00`
   - **Scope options:** All AWS services (Recommended)
   - **Aggregate costs by:** Unblended costs

![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget4.png)
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget5.png)

5. **Alerts** — tạo **3 cảnh báo** ở ngưỡng **60%, 80%, 100%** của budget.
   - **Email recipients:** team-email-1, team-email-2
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget6.png)
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget7.png)
6. **Attach actions – Optional** → **Next**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget8.png)
7. Kiểm tra và **Create**.
![create ps](../../../images/5-Workshop/5.2-Prerequisite/budget9.png)


{{% notice tip %}}
Mọi thứ trong workshop này tính tiền theo giờ (NAT Gateway, RDS, ALB). Tắt/xóa stack khi không dùng — xem [Dọn dẹp](../5.11-Cleanup/).
{{% /notice %}}


