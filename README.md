# AWS Security Monitoring System

A security monitoring system that detects access to sensitive secrets in AWS and sends real-time alerts using  Secrets Manager, CloudTrail, CloudWatch logs & alarms and SNS.

# Purpose & Motivation
Ever wondered how to keep tabs on who’s accessing your most sensitive data in AWS? 
Sensitive information such as API Keys, database credentials, or other critical secrets. Every access to this risk presents a potential security risk.
That is why companies invest heavily in robust monitoring system to track & alert on any unusual activity. But its not just about accessing secrets
In my role as an MCR engineer, modern broadcast environments rely on strict operational accountability and real‑time monitoring & alerting services like dataminer.
Every operational & management action is logged against an authenticated user, ensuring full traceability of activity and reinforcing a culture of accountability across the team. 

This project implements a monitoring and alerting pipeline that detects when my secret in AWS Secrets Manager is accessed (via console or CLI) I will be notified via Amazon SNS.I created this project to  simulate & understand how AWS Security & monitoring services work together to monitor & protect data in production environment.

In this project I will
- Create a secret in Secrets Manager
- set up aws CloudTrail to track secrets access events
- use AWS CloudWatch to log access attempts and trigger notifications
- create SNS alerts to get notified when secrets are accessed.
- build a second notification system & compare which approach delivers better security alerts


# AWS Services Used:
- Secrets Manager - securely store sensitive credentials
- CloudTrail - record API activity made against Secrets Manager
- CloudWatch Logs - Store and analyse CloudTrail events
- CloudWatch Alarms -Metric filters that trigger alerts based on access patterns
- S3 Bucket - long-term storage for CloudTrail logs
- Simple Notification Service (SNS) - & email notification service via E-mail & SMS
- E-mail - Real time alert via SNS



# Architecture

<img width="2172" height="1265" alt="image" src="https://github.com/user-attachments/assets/7b9fbfa2-0f71-4452-b8a4-45d22096194d" />

The solution above has many components, CloudTrail, CloudWatch, alarms, SNS, S3 & Secrets Manager. 

 If i took away CloudWatch & alarms from the solution, what will happen?
I implemented a second approach & configured SNS notifications directly from CloudTrail & compared it against using just CloudWatch alarms.

# Architecture
<img width="1904" height="1104" alt="image" src="https://github.com/user-attachments/assets/a653f51e-f6fb-4f10-859f-442652563b3c" />


# What I learned
- How CloudTrail captures and structures Secrets Manager API events
- How to create CloudWatch metric filters for security monitoring
- How SNS integrates with monitoring services for alerting
- How real-world security teams detect and respond to sensitive data access

# Future Improvements
- Integrate AWS Lambda for advanced event processing
- Add a database service like DynamoDB for auditing purposes
