---
title: "Blog 2"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# PROACTIVE INFRASTRUCTURE MONITORING SOLUTION USING AMBIENT AGENTS ON AMAZON BEDROCK

In large-scale cloud infrastructure management (Multi-Account/Multi-Region), handling a massive volume of alerts (Alert Fatigue) from Amazon CloudWatch often consumes a significant amount of the operations team's time. Traditional rule-based monitoring systems sometimes only detect issues after they have occurred, making it difficult to correlate log and metric data for Root Cause Analysis.

The AgentWatch solution provides a continuously operating Ambient Agent model, assisting engineering teams in gathering infrastructure data, analyzing system behavior, and providing Actionable Insights to optimize operational workflows.

Operational Architecture & Human-in-the-Loop Coordination:

AgentWatch is designed to operate alongside humans by combining automation with a strict approval process (Human-in-the-Loop - HITL), consisting of two main processing flows:

* **Scheduled Ambient Monitoring:** Triggered periodically every 15 minutes by Amazon EventBridge. A LangChain Agent task integrating the Claude model on Amazon Bedrock uses dedicated APIs to query CloudWatch metrics, logs, and alarms from multiple AWS accounts. The LLM then processes the data, extracts key information, and sends a report to the team's Slack channel.
* **On-Demand Interaction & HITL Control:** Engineers can directly query the Agent in real-time via Slack Slash Commands (e.g., `/ask Analyze log patterns for the last hour`). The system categorizes tasks based on risk levels: tasks involving reading information or trend analysis are processed automatically; meanwhile, sensitive infrastructure configuration changes strictly require a 3-step control process: Notify, Question (Clarification), and Review (Pending human approval).

Core AWS Services in the Architecture:

The solution builds an automated workflow (Agentic Workflow) based on the coordination of Cloud-Native services:

* **Amazon Bedrock & Bedrock AgentCore Runtime:** The platform providing the Foundation Model and a secure serverless runtime environment to operate AI Agents at an enterprise scale.
* **Amazon EventBridge & AWS Lambda:** Acts as the scheduler and orchestrator for the Agent's workflow.
* **Amazon CloudWatch & AWS STS:** Provides centralized telemetry data and the assume role mechanism for Cross-Account Monitoring.
* **Amazon API Gateway & Amazon Cognito:** Ensures access control and OAuth 2.0 security for endpoints receiving webhooks from Slack.

Practical Application Benefits:

* **Anomaly Detection Support:** The LLM's Natural Language Understanding (NLU) capabilities help link discrete signals from system logs and metrics, aiding in the detection of anomalies that fixed rule-based configurations might miss.
* **MTTR Reduction Support:** The Agent automatically performs initial context gathering and analysis steps as soon as an alert is detected, providing foundational information to help engineers shorten diagnostic time.
* **Compliance and Governance Assurance:** Reduces repetitive manual tasks for operational engineers while maintaining human control over the Production environment through clear approval steps.

Conclusion: The AgentWatch solution demonstrates the potential of applying Generative AI (Agentic architecture) in optimizing Cloud Operations, shifting monitoring systems from passive information display to more proactive data analysis support.

![Image](/images/3-BlogsPosted/blog2.1.jpg)

- **Post Link:** [AWS Study Group Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2190168085081485)
- **Blog Link:** [AgentWatch: Proactive AWS monitoring with ambient agents](https://aws.amazon.com/vi/blogs/machine-learning/agentwatch-proactive-aws-monitoring-with-ambient-agents/)