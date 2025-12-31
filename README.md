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
<img width="812" height="487" alt="architecture" src="https://github.com/user-attachments/assets/e1c6427f-130e-4644-a7f2-cdd13415f704" />



# What I learned
- CloudTrail captures and structures Secrets Manager API events
- CloudWatch metric filters for security monitoring
- SNS integrates with monitoring services for alerting

# What problem it solves
This project solves the lack of visibility around secrets access in AWS by detecting and alerting on the exact moment sensitive data is retrieved, allowing fast response
It provides real-time visibility and alerts when a secret is accessed by:
- Tracking the exact API call (GetSecretValue) that exposes secret contents
- Recording who accessed the secret, when, and how using CloudTrail
- Converting that event into a security signal using CloudWatch metrics
- Sending an immediate alert via SNS so action can be taken

# Well Architected Framework Alignment

This checklist validates that the security monitoring system follows AWS Well-Architected best practices.

## Security
- Secrets encrypted with KMS
- Secrets access monitored via CloudTrail
- Targeted alerts for `GetSecretValue` events

## Operational Excellence
- CloudWatch Logs enabled for analysis
- Metric filters tested and verified
- Alarms and SNS notifications validated

## Reliability
- Event-driven monitoring design
- End-to-end alert delivery tested

## Cost Optimization
- Alerts scoped to high-risk events only
- No excessive SNS notifications

- 
# Future Improvements
- Integrate AWS Lambda for advanced event processing
- Add a database service like DynamoDB for auditing purposes
