---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Build and finalize critical features on the project's Frontend interface.
* Gain in-depth knowledge of AWS security and identity management by completing practical labs: 000012, 000030, 000044, 000018, 000026, 000033.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date |
| --- | --- | --- | --- |
| Mon | - **Practice Lab 000012:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Access AWS CLI <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Use Time-based access control <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Customer Managed Policies <br> &nbsp;&nbsp;&nbsp;&nbsp;+ IAM Identity Center Identity Store APIs <br> - **Practice Lab 000030:** Limit user permissions using IAM Permission Boundary. | 01/06/2026 | 01/06/2026 |
| Tue | - **Practice Lab 000044:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Create IAM Group <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Create IAM User <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Configure Role Condition <br> - **Frontend:** Code Multi-Tenant login logic (Subdomain input) and auto Refresh Token mechanism. | 02/06/2026 | 02/06/2026 |
| Wed | - **Practice Lab 000018:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Security standards <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Enable Security Hub <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Score for each standard set <br> - **Frontend:** Integrate multi-language support (English/Vietnamese) directly into the UI. | 03/06/2026 | 03/06/2026 |
| Thu | - **Practice Lab 000026:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Use AWS WAF <br> - **Frontend:** Write API connection logic to display HR Management, Attendance, Leave, and Member Administration functionalities. | 04/06/2026 | 04/06/2026 |
| Fri | - **Practice Lab 000033:** <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Create Key Management Service <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Create Amazon S3 <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Create AWS CloudTrail and Amazon Athena <br> &nbsp;&nbsp;&nbsp;&nbsp;+ Test and share encrypted data on S3 | 05/06/2026 | 05/06/2026 |
| Weekend | - **Frontend:** Build the Health Check interface to monitor microservices| 06/06/2026 | 07/06/2026 |

### Week 7 Achievements:

* **Frontend Development Progress:**
    * Successfully implemented Multi-Tenant login logic requiring Subdomain input, combined with an automatic Refresh Token mechanism to securely maintain user sessions.
    * Integrated multi-language support (English/Vietnamese) directly into the UI, improving user experience.
    * Completed API integration logic to smoothly display critical modules: HR Management, Attendance Tracking, Leave Requests, and Member Administration.
    * Designed and built the Health Check interface to continuously monitor the operational status of the system's microservices.
* **AWS Practical Labs (Security & Identity):**
    * **Lab 000012 (IAM Identity Center):** Learned to centrally manage workforce identities across multiple AWS accounts following the principle of least privilege.
    * **Lab 000030 (IAM Permission Boundary):** Understood how to set up Permission Boundaries to limit the maximum permissions of a user/group, preventing privilege escalation risks.
    * **Lab 000044 (IAM Role & Condition):** Practiced creating Roles and enhancing security by configuring access conditions based on IP addresses and time limits.
    * **Lab 000018 (AWS Security Hub):** Explored the comprehensive security dashboard to view high-priority alerts and automate compliance checks.
    * **Lab 000026 (AWS WAF):** Learned how to use the Web Application Firewall to protect systems from common vulnerabilities like SQL Injection and Cross-Site Scripting (XSS).
    * **Lab 000033 (AWS KMS):** Practiced encrypting S3 Objects at rest using the Key Management Service (KMS), logging activities with CloudTrail, and querying data with Athena.

### Screenshots

![Multi-Tenant Login Interface](/images/1-Worklog/login.png)
![Multi-language Interface - Vietnamese](/images/1-Worklog/VIE.png)
![Multi-language Interface - English](/images/1-Worklog/eng.png)
![Employee Management Interface](/images/1-Worklog/staff.png)
![System Health Check Interface](/images/1-Worklog/healthcheck.png)