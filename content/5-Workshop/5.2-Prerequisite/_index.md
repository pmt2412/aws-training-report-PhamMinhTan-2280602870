---
title: "Prerequisite"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Prerequisites

| Requirement | Detail |
|:--|:--|
| **AWS account** | One shared account. Root locked, MFA on. |
| **Region** | `ap-southeast-1` (Singapore). `us-east-1` only for the CloudFront ACM cert (if a custom domain is used). |
| **Team access** | **IAM Identity Center** — one SSO user per team member (no shared root, no static keys). |
| **AWS CLI** | Configured via IAM Identity Center (for ECR push, S3 sync). |
| **Docker** | To build and push the 3 service images. |

---

## 1. Team access with IAM Identity Center

The team works on one shared AWS account. Instead of sharing root or handing out access keys, each member gets their **own SSO user** through **IAM Identity Center**, grouped and granted access with a permission set.

> ℹ️ These SSO users are for **humans building the stack** (console + CLI). They are separate from the **application's runtime IAM roles**, which stay least-privilege — see [Security & IAM](../5.10-Security-IAM/).

#### 1.1. Open IAM Identity Center
Search for and open **IAM Identity Center** in the console, then enable it.

![search IIC](../../images/5-Workshop/5.2-Prerequisite/search-iic.png)

#### 1.2. Create users and groups
1. Left menu → **Users → Add user**. Fill the required fields (Username, Email, First name, Last name) → **Next** to finish.
![add user](../../images/5-Workshop/5.2-Prerequisite/add-user.png)
![add user](../../images/5-Workshop/5.2-Prerequisite/add-user1.png)
2. **Groups → Create group** — name the group and add the users you just created (easier to manage).
![create group](../../images/5-Workshop/5.2-Prerequisite/create-group.png)


#### 1.3. Create a permission set
1. **Permission sets → Create permission set**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/create-permission-set.png)
2. Permission set type = **Predefined permission set**.
3. Policy = **AdministratorAccess**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/create-permission-set1.png)

4. Set a **name** and a **Session duration**, then **Create**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/create-permission-set2.png)
![create ps](../../images/5-Workshop/5.2-Prerequisite/create-permission-set3.png)
> 📷 **[Screenshot]** Predefined = AdministratorAccess → `/images/5-Workshop/5.2-Prerequisite/iic-permset.png`

#### 1.4. Grant access to the AWS account
1. Left menu → under **Multi-account permissions**, choose **AWS accounts**.
2. Tick the AWS account to grant access to → **Assign users or groups**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-account.png)
3. Choose the user/group you created → **Next**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-account2.png)

4. Choose the permission set → **Next**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-account3.png)

5. Review and **Submit**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-account4.png)
![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-account5.png)


#### 1.5. Sign in via the AWS access portal
Back on the IAM Identity Center **Dashboard**, open the **AWS access portal URL** and sign in with the SSO user.
![create ps](../../images/5-Workshop/5.2-Prerequisite/open-link1.png)
![create ps](../../images/5-Workshop/5.2-Prerequisite/sign-in.png)


---

## 2. Configure the AWS CLI (via IAM Identity Center)

Connect the CLI to the access portal so it uses short-lived SSO credentials (no static keys) — needed for ECR push and S3 sync.

```bash
aws configure sso
# SSO start URL:   https://<your-portal>.awsapps.com/start
# SSO Region:      ap-southeast-1
# (browser opens → approve) → pick account + permission set
# CLI default Region: ap-southeast-1
# CLI profile name:   saashr

# start / refresh a session:
aws sso login --profile saashr
aws sts get-caller-identity --profile saashr
```

![create ps](../../images/5-Workshop/5.2-Prerequisite/aws-conf.png)

> ⚠️ **Verify:** confirm these CLI steps match what you actually ran (from the earlier setup). If you used a different profile name or method, tell me and I'll adjust.

---

## 3. Docker
Install Docker Desktop (or Docker Engine) to build and push the `auth` / `tenant` / `hr` images to ECR in [Application Tier](../5.6-Compute-ECS/).

---

## Step 0 — Cost safety net (AWS Budgets)
Set up a budget with email alerts so cost overruns are caught early (free).

1. Search for and open **Billing and Cost Management → Budgets**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget1.png)
2. Click **Create budget**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget2.png)


3. **Budget setup = Customize (advanced)**; **Budget type = Cost budget – Recommended** → **Next**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget3.png)
4. Configure the budget:
   - **Budget name:** `SaaS-HR_Multi-tenant-Budget`
   - **Period:** Custom — Start `2026/06/27`, End `2026/07/27`
   - **Budgeted amount:** `130.00`
   - **Scope options:** All AWS services (Recommended)
   - **Aggregate costs by:** Unblended costs

![create ps](../../images/5-Workshop/5.2-Prerequisite/budget4.png)
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget5.png)

5. **Alerts** — create **3 alerts** at thresholds **60%, 80%, 100%** of the budget.
   - **Email recipients:** team-email-1, team-email-2
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget6.png)
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget7.png)
6. **Attach actions – Optional** → **Next**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget8.png)
7. Review and **Create**.
![create ps](../../images/5-Workshop/5.2-Prerequisite/budget9.png)


{{% notice tip %}}
Everything in this workshop bills by the hour (NAT Gateway, RDS, ALB). Tear the stack down when idle — see [Clean-up](../5.11-Cleanup/).
{{% /notice %}}
