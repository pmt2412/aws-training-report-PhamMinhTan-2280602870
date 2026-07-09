---
title: "Blog 3"
date: 2026-06-26
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---


# VPC LINK IN PRIVATE INTEGRATIONS OF AMAZON API GATEWAY

VPC link is a resource in Amazon API Gateway that allows connecting API routes to internal, private resources located within a VPC network. The article delves into the core technologies behind VPC Link (such as AWS Hyperplane and AWS PrivateLink) and compares the fundamental architectural differences between VPC Link for REST APIs and HTTP APIs.

Core foundational technologies:

* **AWS Hyperplane:** AWS's internal network virtualization platform that supports connection and routing between different VPCs. Hyperplane uses VPC-to-VPC NAT instead of PrivateLink.
* **AWS PrivateLink:** Technology that allows accessing AWS services privately within the AWS internal network without routing through the public Internet. Traffic is routed through Interface VPC Endpoints (consumer side) and VPC Endpoint Services (provider side).

Distinguishing Private API and Private Integration:

* **Private API:** Means the API Gateway Endpoint can only be accessed from within a VPC (or via Direct Connect/VPN). Currently, only REST APIs support configuring Private APIs.
* **Private Integration:** Means the backend system (behind the API Gateway) is located within a VPC and is not publicly exposed to the Internet. API Gateway uses VPC Link to connect to this backend securely.

Comparing VPC Link for REST API vs. HTTP API:

* **Underlying technology:** The first difference lies in the underlying technology. VPC Link for REST APIs is based on AWS PrivateLink, while HTTP APIs use the VPC-to-VPC NAT network address translation system of AWS Hyperplane. This difference leads to major architectural changes underneath.
* **Resources and integration capabilities:** HTTP APIs are significantly more flexible as they do not require a VPC Endpoint Service and can connect directly to ALBs, NLBs, or services via AWS Cloud Map; a single VPC Link can also associate with multiple backends. Conversely, REST APIs strictly require a VPC Endpoint Service, connect through a single NLB, and configuring multi-backends is much more complex.
* **Features and IP addresses:** HTTP APIs leverage the Layer 7 power of ALBs (like advanced routing, authentication) and operate via an ENI tunneling mechanism. Meanwhile, REST APIs are more restricted at Layer 7 since they route through an NLB (Layer 4), and the backend system will identify the source IP as the private IP of the NLB itself.

Understanding the differences between the two types of VPC Links helps architectural engineers make accurate decisions when designing systems. If you need a more cost-effective, faster API solution with flexible integration with ALBs or containers, VPC Link for HTTP APIs is the optimal choice. If you require the distinct, in-depth features of REST APIs, you will need to configure VPC Link via the PrivateLink mechanism with an NLB.

![Image 1](/images/3-BlogsPosted/blog3.1.jpg)
![Image 2](/images/3-BlogsPosted/blog3.2.jpg)
![Image 3](/images/3-BlogsPosted/blog3.3.jpg)

- **Post Link:** [AWS Study Group Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2194084638023163)
- **Blog Link:** [Understanding VPC links in Amazon API Gateway private integrations](https://aws.amazon.com/vi/blogs/compute/understanding-vpc-links-in-amazon-api-gateway-private-integrations/?fbclid=IwY2xjawS3BcdleHRuA2FlbQIxMABicmlkETFWanVNbWhjdjRGR0g4NEFxc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHnd0siHru9JZp4Q6bMUgKNmulHBR7GCnCVUQsPz38_-8DjQKWSt0fGK1uS9s_aem_8iC52_t_hIPs-EySNErBaA)