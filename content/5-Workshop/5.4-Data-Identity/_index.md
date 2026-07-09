---
title: "Data & Identity"
date: 2026-07-08
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

The data tier (RDS), the secrets store (SSM), and the identity setup (Cognito pool + the app's RS256 JWT keys).

## Step 3 — RDS MySQL Multi-AZ

1. **RDS → Subnet groups** → `saashr-db-subnets` = `data-1a` + `data-1b`.
2. **Create database** → MySQL → `db.t4g.micro` → **Multi-AZ: Yes (create a standby)**.
- Search Aurora and RDS -> Databases -> Click Create Database -> Choose Full Configuration
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db.png)
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db1.png)

![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db3.png)
3. Storage type: gp3; Storage 20 GB gp3; Enable Storage Autoscaling: Untick.
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db4.png)
4. VPC = `saashr-vpc`, subnet group = `saashr-db-subnets`, **Public access = No**, SG = `sg-rds`.
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db5.png)
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db6.png)

- Database create successfully
![Create db](../../images/5-Workshop/5.4-Data-Identity/create-db7.png)
5. After creation, run `database/init.sql` to create `auth_db`, `tenant_db`, `hr_db` (via a temporary bastion).
    - create ec2
![Create db](../../images/5-Workshop/5.4-Data-Identity/test-db2.png)
    - Cut the init.sql file into ec2
![Create db](../../images/5-Workshop/5.4-Data-Identity/test-db3.png)

    - Connect ec2 to database
![Create db](../../images/5-Workshop/5.4-Data-Identity/test-db1.png)


> 📎 **Attachment:** `database/init.sql` (place under `5.4-Data-Identity/files/`).

## Step 4 — Secrets (SSM Parameter Store)

Store configuration and secrets in **Systems Manager → Parameter Store** as **SecureString** (free), later referenced by the ECS task definition — nothing is hard-coded in the images.

1. Search for and open **Systems Manager → Parameter Store**.
2. Click **Create parameter**.

![Create SSM PS](../../images/5-Workshop/5.4-Data-Identity/ssm1.png)
3. For each parameter below: enter the **Name**, set **Type = SecureString**, paste the **Value**, then **Create parameter**. Repeat for all 7.
![Create SSM PS](../../images/5-Workshop/5.4-Data-Identity/ssm2.png)
| Parameter name | Holds |
|:--|:--|
| `/saashr/cognito/client_id` | Cognito app client ID |
| `/saashr/cognito/pool_id` | Cognito user pool ID |
| `/saashr/db/host` | RDS endpoint (host) |
| `/saashr/db/password` | RDS database password |
| `/saashr/jwt/private_key` | RS256 JWT signing private key |
| `/saashr/jwt/public_key` | RS256 JWT verification public key |
| `/saashr/sqs/url` | SQS queue URL |

- On AWS Console -> click Cloudshell at the bottom -> patse this command to get private key and public key 
#### Generating the RS256 JWT keypair
`/saashr/jwt/private_key` and `/saashr/jwt/public_key` are an RSA keypair. Generate them once in **AWS CloudShell**, then paste the PEM output into the two SecureString parameters above:

```python
python3 -c "
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import serialization

key = rsa.generate_private_key(public_exponent=65537, key_size=2048)

priv = key.private_bytes(serialization.Encoding.PEM, serialization.PrivateFormat.TraditionalOpenSSL, serialization.NoEncryption()).decode()
pub = key.public_key().public_bytes(serialization.Encoding.PEM, serialization.PublicFormat.SubjectPublicKeyInfo).decode()

print('=== PRIVATE KEY ===')
print(priv)
print('=== PUBLIC KEY ===')
print(pub)
"
```

> ⚠️ **Never publish the real private key.** Show only this command — redact the printed key and mask the `/saashr/jwt/private_key` value in any screenshot. Anyone holding the private key can forge JWTs for the app.

{{% notice tip %}}
`db/password` and `jwt/private_key` must be **SecureString**. The same values can be set from the CLI, e.g. `aws ssm put-parameter --name "/saashr/db/host" --type SecureString --value "<rds-endpoint>"`.
{{% /notice %}}


## Step 5 — Cognito

1. Search for and open **Cognito → User pools**.

2. Click **Create user pool** and configure:
   - **Application type:** Traditional web application
   - **Name your application:** `saashr-app`
   - **Sign-in identifiers:** Email
   - **Self-registration:** Enable self-registration
   - **Return URL:** `https://localhost` (dev; use the CloudFront URL in production)

   Then click **Create user directory** to create the pool.



3. Open the new user pool → **App clients**.
4. Click **Create app client**:
   - **Application type:** Traditional web application
   - **Name your application:** `saashr-app`
   - **Return URL:** `https://localhost`

   Then click **Create app client** to finish.



5. Note the **User pool ID** and **App client ID** → store them in SSM as `/saashr/cognito/pool_id` and `/saashr/cognito/client_id` (Step 4).

{{% notice note %}}
This pool provides the **user directory and login**. Tenant isolation (`tenant_id`) and roles are carried in the application's **RS256 JWT** (keys from Step 4).
{{% /notice %}}
