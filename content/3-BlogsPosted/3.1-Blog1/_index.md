---
title: "Blog 1"
date: 2026-06-19
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# SECURELY PROVIDING DATABASE CREDENTIALS TO LAMBDA FUNCTIONS USING AWS SECRETS MANAGER

Using AWS Secrets Manager allows you to secure database credentials and pass them to AWS Lambda functions when connecting to Amazon RDS (MySQL). This solution eliminates the need to hardcode passwords in source code or pass them via environment variables, thereby protecting the backend database more securely. Additionally, the automatic periodic password rotation feature helps minimize security risks.

The operational model of the solution includes

* The client sends a Request to a RESTful API hosted on AWS API Gateway.
* API Gateway executes the corresponding AWS Lambda function.
* The Lambda function calls the AWS Secrets Manager API to retrieve database login information (username/password).
* The Lambda function uses that information to connect, query the Amazon RDS (MySQL) database, and return the results.

Deployment process via CloudFormation:

An AWS CloudFormation template is used to automate the provisioning of:

* An RDS MySQL database (db.t3.micro instance type).
* Two Lambda functions: One (LambdaRDSCFNInit) to create an Employees table and insert sample data immediately after stack creation; another (LambdaRDSTest) to query the employee count.
* A RESTful API Gateway with a GET method.
* A Secret resource in Secrets Manager with a randomly generated password.

Key technical points:

* **Dynamic References:** CloudFormation uses dynamic references to retrieve the password from Secrets Manager when creating the RDS instance. This ensures CloudFormation does not log or store the password in plain text.
* **Automatic Rotation:** The AWS SecretsManager RotationSchedule resource is configured in coordination with a rotation Lambda function to automatically change the RDS database password every 30 days.

Combining Lambda with AWS Secrets Manager helps automatically manage the lifecycle of sensitive information, reduces the operational cost of dedicated security infrastructure, and significantly enhances the security of Serverless applications.

![Image](/images/3-BlogsPosted/blog1.1.jpg)

- **Post Link:** [AWS Study Group Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2187144322050528)
- **Blog Link:** [How to securely provide database credentials to Lambda functions by using AWS Secrets Manager](https://aws.amazon.com/vi/blogs/security/how-to-securely-provide-database-credentials-to-lambda-functions-by-using-aws-secrets-manager/?fbclid=IwY2xjawS3AUNleHRuA2FlbQIxMABicmlkETFWanVNbWhjdjRGR0g4NEFxc3J0YwZhcHBfaWQQMjIyMDM5MTc4ODIwMDg5MgABHu4APzf301MYy7P9_61k2xiY8s_uPcppPQp_j0D1ebu-DsVcDHbhTa6vYkDd_aem_n-kVGqUxYdH_v_p1Btu0RA)