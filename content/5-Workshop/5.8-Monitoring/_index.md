---
title: "Monitoring & Alerts"
date: 2026-07-08
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

**Goal:** monitor the ECS services — when a service's CPU Utilization reaches **≥ 70%**, CloudWatch triggers an alarm and emails the team through Amazon SNS.

## Step 7 — Amazon SNS (topic + email subscription)

#### 7.1 Create the SNS topic
**Amazon SNS → Topics → Create topic.**
- **Type:** Standard
- **Name:** `saashr-alerts`
- **Display name:** `saashr-alerts`
- Click **Create topic**.

![Create SNS topic](../../images/5-Workshop/5.8-Monitoring/sns1.png)

#### 7.2 Create the email subscription
Open the `saashr-alerts` topic → **Subscriptions** tab → **Create subscription**.
- **Protocol:** Email
- **Endpoint:** `<team-email>`
- Click **Create subscription**.

![Create email subscription](../../images/5-Workshop/5.8-Monitoring/sns2.png)

#### 7.3 Confirm the email
Amazon SNS sends a confirmation email to that address. Open it and click **Confirm subscription**. The status changes from **Pending confirmation** to **Confirmed**.



![Subscription confirmed](../../images/5-Workshop/5.8-Monitoring/sns3.png)

## Step 13 — CloudWatch Alarm → SNS

#### 13.1 Create alarm → select metric
**CloudWatch → Alarms → Create alarm** → **Select metric**.

![Create alarm](../../images/5-Workshop/5.8-Monitoring/cw1.png)

#### 13.2 Choose the ECS metric
In **Select metric**, follow **ECS → ClusterName, ServiceName**:
- **Cluster:** `saashr-cluster2`
- **Service:** `saashr-hr-svc`
- **Metric:** `CPUUtilization`
- Click **Select metric**.

![ECS CPUUtilization metric](../../images/5-Workshop/5.8-Monitoring/cw2.png)

#### 13.3 Configure the condition
- **Statistic:** Average
- **Period:** 1 minute
- **Threshold type:** Static
- **Whenever CPUUtilization is:** Greater/Equal
- **Threshold value:** `70`

The alarm enters the **ALARM** state when CPU Utilization is ≥ 70% over a 1-minute period.

![Threshold config](../../images/5-Workshop/5.8-Monitoring/cw3.png)

#### 13.4 Configure the action
- **Alarm state trigger:** In alarm
- **Notification → Select an existing SNS topic → `saashr-alerts`**

CloudWatch emails every confirmed subscriber of the topic when the alarm fires.

![Link alarm to SNS](../../images/5-Workshop/5.8-Monitoring/cw4.png)

#### 13.5 Name the alarm
- **Alarm name:** `saashr-alerts-alarm-noti` (add a description if needed)
- Click **Next → Create alarm**.

![Name the alarm](../../images/5-Workshop/5.8-Monitoring/cw5.png)

#### 13.6 Verify
The new alarm appears with:
- **Metric:** `CPUUtilization` · **Threshold:** ≥ 70% · **Action:** SNS Notification
- **State:** OK (or **Insufficient data** at first)

Open it to watch the CPU Utilization graph, the threshold line, and the alarm history.

![Alarm created](../../images/5-Workshop/5.8-Monitoring/cw6.png)

{{% notice note %}}
This alarm watches one service (`saashr-hr-svc`). Repeat 13.1–13.5 for `auth` and `tenant` to cover all three.
{{% /notice %}}

#### Logs
Each service also streams logs to the CloudWatch Logs group `/ecs/saashr/{service}` (configured in the ECS task definition, Step 10).

![Log group](../../images/5-Workshop/5.8-Monitoring/cw7.png)
![Log group](../../images/5-Workshop/5.8-Monitoring/cw8.png)
