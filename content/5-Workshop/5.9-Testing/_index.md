---
title: "Testing & Validation"
date: 2026-07-08
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

End-to-end validation of the deployed stack via the CloudFront entry point, followed by error testing, high-availability testing, and observability. Base URL:

```bash
BASE="https://d3htvmot332c6v.cloudfront.net/"   
```

## 1. Health checks (smoke test)

```bash
curl -s $BASE/api/v1/auth/health
curl -s $BASE/api/v1/tenants/health
curl -s $BASE/api/v1/hr/health
```
**Result — all three healthy (the `database:up` fields confirm ECS *and* RDS are reachable):**
```json
{"status":"healthy","service":"auth-service"}
{"status":"healthy","database":"up","service":"tenant-service"}
{"status":"healthy","database":"up","service":"hr-service"}
```

![Health checks](../../images/5-Workshop/5.9-Testing/curl1.png)

## 2. Frontend delivery

```bash
curl -I $BASE/
```
**Result:** `HTTP/2 200`, `content-type: text/html` — the React SPA is served through CloudFront (from the private S3 origin).



## 3. Authentication — end-to-end

```bash
# Register a workspace + owner
curl -s -X POST $BASE/api/v1/auth/register -H "Content-Type: application/json" \
  -d '{"email":"owner@acmedemo.com","password":"Passw0rd!","tenant_name":"Acme Demo","subdomain":"acmedemo"}'

# Log in → receive an access token
curl -s -X POST $BASE/api/v1/auth/login -H "Content-Type: application/json" \
  -d '{"email":"owner@acmedemo.com","password":"Passw0rd!","subdomain":"acmedemo"}'

# Verify the token
curl -s $BASE/api/v1/auth/me -H "Authorization: Bearer $TOKEN"
```
**Result — `/me` returns the authenticated identity with the correct role:**
```json
{"id":"usr_9476d71e-...","email":"owner@acmedemo.com","status":"active","active_tenant_id":"tenant_369f39d7-...","role":"owner"}
```
The token is an **app-signed RS256 JWT** (auth-service signs with the private key; the other services verify with the public key — see [Data & Identity](../5.4-Data-Identity/)). Its claims (`tenant_id`, `role`) drive every downstream authorization check.

![Frontend loads](../../images/5-Workshop/5.9-Testing/curl2.png)

## 4. Business operations (authenticated, tenant-scoped)

```bash
curl -s $BASE/api/v1/tenants/my-tenant -H "Authorization: Bearer $TOKEN"
curl -s -X POST $BASE/api/v1/hr/departments -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" -d '{"name":"Engineering"}'
curl -s $BASE/api/v1/hr/employees -H "Authorization: Bearer $TOKEN"
```
**Result:**
```json
{"id":"tenant_369f39d7-...","name":"Acme Demo","subdomain":"acmedemo","status":"active","plan_tier":"basic","created_at":"2026-07-08T00:24:15"}
{"id":"dept_bcd98e2a-...","tenant_id":"tenant_369f39d7-...","name":"Engineering","description":null,"created_at":"2026-07-08T00:27:25"}
[]
```
Reading the tenant, creating a department (`201`), and listing employees (empty for a new tenant) all succeed — data reads/writes to RDS work through the authenticated path.

![Business operations](../../images/5-Workshop/5.9-Testing/curl4.png)

## 5. Validation & error testing
{{% notice info %}}
API error responses are tested **directly against the ALB** (`http://<alb-dns>/...`). CloudFront's SPA custom error responses map `403/404` to `index.html` (`200`), which masks API error codes for browser clients — so the API tier is validated at the ALB, where `server: uvicorn` confirms the request reached the application.
{{% /notice %}}

| Test | Command | Observed result |
|:--|:--|:--|
| No token | `curl -i $ALB/api/v1/hr/employees` | **`403 Forbidden`** `{"detail":"Not authenticated"}` ✅ |
| Wrong password | login with a bad password | **`401 Unauthorized`** `{"detail":"Invalid email or password"}` ✅ |
| Duplicate subdomain | register an existing subdomain | **`409`** `{"detail":"Tenant subdomain already registered."}` ✅ |
| Duplicate department | create a department with an existing name | Blocked by the DB unique constraint `uq_tenant_dept` (see note) |
| Tenant isolation | register tenant **B**, log in as B, `GET /api/v1/hr/employees` | Returns **only B's** data — A's records are never visible |

**Observed — duplicate subdomain is rejected:**
```json
{"detail":"Tenant subdomain already registered."}
```

{{% notice note %}}
**Known limitation:** a duplicate department name is correctly prevented at the database level (unique key `uq_tenant_dept`), but the API currently surfaces it as **HTTP 500** instead of a clean **409 Conflict**. The data integrity is safe; only the error response needs improving.
{{% /notice %}}

![Business operations](../../images/5-Workshop/5.9-Testing/curl5.png)
![Business operations](../../images/5-Workshop/5.9-Testing/tenant-isolution.png)

## 6. Async messaging (SQS) — testing finding

End-to-end testing of the asynchronous path (tenant status change → SQS → hr-service) surfaced an **integration issue in the current deployment**, which is exactly what this stage of testing is meant to catch:

- **Infrastructure is healthy** — a message sent manually to `saashr-events` was accepted (the *NumberOfMessagesSent* / *SentMessageSize* metrics increased), confirming the queue and DLQ are correctly provisioned.
- **Producer** — `tenant-service` did **not** publish to the queue on a status change (no *MessagesSent* from the app), despite the task role holding `sqs:SendMessage`.
- **Consumer** — the queued test message was **not drained** (`MessagesVisible = 1`, `MessagesDeleted = 0`), so the consumer was not processing during the test window.

**Assessment:** the event-driven **design** (SQS decoupling + DLQ + long-polling consumer, see [Async Messaging](../5.5-Async-SQS/)) is sound; the gap is in the deployment wiring (likely a stale service image and/or consumer startup), and the fix is in progress. This is recorded here as an honest testing finding rather than claimed as passing.

![SQS metrics](../../images/5-Workshop/5.9-Testing/sqs-metrics.png)

## 7. Observability

- **Logs:** CloudWatch Logs groups `/ecs/saashr-auth`, `/ecs/saashr-tenant`, `/ecs/saashr-hr` capture request logs and unhandled errors.
- **Metrics:** CloudWatch → ECS `CPUUtilization` per service.
- **Alert → email:** trigger the alarm without generating real load:
  ```bash
  aws cloudwatch set-alarm-state --alarm-name saashr-alerts-alarm-noti \
    --state-value ALARM --state-reason "manual test"
  ```
  **Expected:** the SNS email arrives within seconds and the alarm shows the **ALARM** state (see [Monitoring & Alerts](../5.8-Monitoring/)).

![Alarm email](../../images/5-Workshop/5.9-Testing/alarm1.png)

![Alarm email](../../images/5-Workshop/5.9-Testing/alarm2.png)

![Alarm email](../../images/5-Workshop/5.9-Testing/alarm3.png)