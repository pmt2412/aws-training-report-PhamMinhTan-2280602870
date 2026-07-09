---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “FCAJ Community Day” (May 23)

### Event Objectives

- Update on the latest trends and mindsets regarding Artificial Intelligence (LLMs, Prompting, Multi-Agent).
- Gain deep insight and hands-on optimization practice with AWS services (Amazon CloudFront, Amazon Quick).
- Share hands-on experience from Hackathons and the process of building real-world products.
- Assess current market demands and personal value orientation.

### Speakers

- **Tinh Truong** - Shared the importance of Context in AI.
- **Anh Pham** - Introduced Friendly AI Assistant with Amazon Quick.
- **Thinh Nguyen** - Optimization and Security with Amazon CloudFront.
- **Team VIB** - Shared their 36-hour journey at LotusHacks.
- **Duc Dao** - Discussed the Non-Determinism of LLMs.
- **Vy Lam** - Enterprise-Grade Multi-Agent Systems.
- **Anh Hung** - Shared insights on market demand and personal direction.

### Key Highlights

#### 1. Context Is Everything & Prompting Mindset (Tinh Truong & Anh Hung)
- To create personal value in the market, one must know how to use AI effectively. AI fails without context.
- Introduced the concept of the **Second AI Brain** (using the Obsidian tool).
- **Principles for writing standard prompts:**
  - Only feed the AI the context it actually needs; do not supply redundant information it already knows, and do not randomly compile everything on the Internet into the prompt.
  - Do not over-describe the obvious. Instead, explicitly specify **what problem to solve next**.

#### 2. Friendly AI Assistant with Amazon Quick (Anh Pham)
- AWS provides a platform connecting the ecosystem for users to build their own products.
- Outstanding features: **Quick Chat Agent** (explore data, analyze insights), **Quick Flows** (create smart workflows using natural language - no coding), **Quick Spaces** (team workspace), **Quick Sight** (create dashboards from raw data).
- **Real-world Demo:** Using AI to analyze insights from an Excel file; creating an Agent to automatically summarize meeting notes and email them to everyone.

#### 3. Amazon CloudFront as Your Foundation (Thinh Nguyen)
- Diverse target audience from small website owners to large enterprises with a new pricing mechanism (easy to estimate, fixed-price CDN).
- **Security Capabilities:** Support for HTTPS, Mutual TLS, Origin Cloaking (creating a VPC tunnel from CloudFront to Origin), Viewer access control (geo-blocking), Content protection with Signed URLs (preventing copy-pasting links).
- **Architecture & Performance:** Multi-layer caching, HTTP/3 (QUIC/UDP) multiplexing, maintaining persistent connections to origins to avoid repetitive TCP handshakes.
- Intelligent routing capabilities with **CloudFront Functions**.

#### 4. The 36-Hour Journey at LotusHacks (Team VIB)
- The process from "Zero to Idea", brainstorming, and defining the problem to build the **UTMorpho** application within 36 hours of continuous development.
- App demo allowing direct interaction with components to edit via 3 AI agents, with the ability to review modification history.

#### 5. The "Non-Determinism" Nature of LLMs (Duc Dao)
- The mechanism of an LLM is to rank scores for words in the prompt to select the most appropriate word from its vocabulary.
- **Reality:** Many mistakenly believe that setting `Temperature = 0` guarantees determinism. However, current LLM providers employ inference optimization methods to optimize costs (plan pricing), meaning the output can still vary.

#### 6. Enterprise-Grade Multi-Agent System (Vy Lam)
- **Case Study:** Startup Credit Scoring. The challenge lies in startup data not matching traditional banking structures.
- **Multi-Agent Model:** Acts as a virtual board of experts with specialized roles: Financial analyst, Market analyst, Team evaluator, Risk assessor, Compliance.
- **Why this architecture works effectively:** Specialized expertise, checks & balances, parallel processing, auditability, scalability, and fault tolerance. The system must ensure security, reliability, and compliance (Guardrails & Compliance).

### Key Takeaways & Application

- **Reshaping communication mindset with AI:** Recognizing the power of "Context". In my daily work, I will apply the principle of providing just enough context, avoiding information noise, and building a "Second AI Brain" to store and systemize personal knowledge intelligently.
- **Optimizing AWS network infrastructure:** Grasping the advanced security features of CloudFront (Origin Cloaking, Signed URLs) and persistent connection mechanisms. This is highly useful for designing secure and faster web architectures.
- **Advanced AI system design:** Clearly acknowledging the limitations of a Single Agent and understanding how to design a Multi-Agent model to solve complex problems requiring checks and balances, especially in strict enterprise workflows.
- **Deeper understanding of LLMs:** Understanding the non-deterministic nature of LLMs even when `Temperature = 0`, allowing for appropriate error handling (mitigation strategies) when integrating AI into practical applications.

#### Event Photos
![Event check-in](/images/4-EventParticipated/checkin1.png)
*Event check-in proof*

![Presentation photo](/images/4-EventParticipated/event1.jpg)
*Speaker Duc Dao presenting on the Non-Determinism of LLMs*

> Attending FCAJ Community Day not only brought me the latest technology updates on AWS and GenAI but also provided practical insights from enterprise problem-solving, helping me better orient my future professional development path.
