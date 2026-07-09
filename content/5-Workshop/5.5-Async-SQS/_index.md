---
title: "Async Messaging (SQS)"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

`tenant-service` publishes business events; `hr-service` consumes them asynchronously. The queue decouples the two and absorbs spikes; a Dead-Letter Queue catches poison messages.

## Step 6 — SQS + DLQ

1. Search for and open **Simple Queue Service → Queues**.

![Create sqs](../../images/5-Workshop/5.5-SQS/sqs.png)

2. **Create the main queue `saashr-events`:**
   - **Type:** Standard
   - **Name:** `saashr-events`
   - Keep the default configuration, but set **Receive message wait time = 20** (enables long-polling — matches the consumer).
   - **Access policy:** Advanced
   - **Dead-letter queue** → *"Set this queue to receive undeliverable messages"* = **Enabled**
     - **Choose queue:** `arn:aws:sqs:ap-southeast-1:<account-id>:saashr-events-dlq`
     - **Maximum receives:** `5`
   - Click **Create queue**.

![Create sqs](../../images/5-Workshop/5.5-SQS/sqs1.png)
![Create sqs](../../images/5-Workshop/5.5-SQS/sqs2.png)
![Create sqs](../../images/5-Workshop/5.5-SQS/sqs3.png)
3. **Create the dead-letter queue `saashr-events-dlq`:**
   - **Type:** Standard
   - **Name:** `saashr-events-dlq`
   - **Access policy:** Advanced
   - Click **Create queue**.
![Create sqs](../../images/5-Workshop/5.5-SQS/sqs4.png)
![Create sqs](../../images/5-Workshop/5.5-SQS/sqs5.png)
![Create sqs](../../images/5-Workshop/5.5-SQS/sqs6.png)

4. Note the **`saashr-events` Queue URL** → save into SSM as `/saashr/sqs/url` (Step 4).

{{% notice note %}}
The console only lets you select a DLQ that **already exists**. If so, create `saashr-events-dlq` first, then enable the redrive policy on `saashr-events`.
{{% /notice %}}

#### Application code (already implemented)
Publisher — `tenant-service/app/core/sqs.py` (boto3, uses the ECS task-role credentials, no static keys):
```python
# send_message(QueueUrl=SQS_QUEUE_URL, MessageBody=json.dumps(event))
```
Consumer — `hr-service/app/core/worker.py` (`start_sqs_consumer`): long-poll → process → `delete_message` on success (failures retry, then land in the DLQ).

> 📎 **Attachment:** `sqs.py`, `worker.py` (place under `5.5-Async-SQS/files/`).

{{% notice info %}}
Event-driven decoupling via SQS is the scalability story for the async path — see also [Security & IAM](../5.10-Security-IAM/) for the task-role SQS permissions.
{{% /notice %}}