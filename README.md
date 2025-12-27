# Security Monitoring Tool

Ever wondered how to keep tabs on who’s accessing your most sensitive data in AWS? 
it could be API Keys, database credentials, or other critical secrets. its a security risk every time someone accesses confidential information. 
That is why companies invest heavily in robust monitoring system to track & alert on any unusual activity. But its not just about accessing secrets
In my role as an MCR engineer, modern broadcast environments rely on strict operational accountability and real‑time service assurance.
every operational & management action is logged against an authenticated user, ensuring full traceability of activity and reinforcing a culture of accountability across the team. 
This exposure has the foundation of the AWS Security Monitoring System designed in this project.

The system continuously evaluates key performance and security metrics, triggering automated alarms whenever thresholds are breached—similar to how broadcast monitoring alerts us to service degradation in television workflows. 
By combining detailed user‑action auditing with proactive alerting, this AWS‑based solution delivers a robust, transparent, and highly responsive security

In this project I will
- Create a secret in Secrets Manager
- set up aws CloudTrail to track secrets access events
- use AWS CloudWatch to log access attempts and trigger notifications
- create SNS alerts to get notified when secrets are accessed.
- build a second notification system & compare which approach delivers better security alerts

# AWS Services Used:
- Secrets Manager - store a secret to protect
- CloudTrail - record API activity detect GetSecretValue
- CloudWatch [Logs & Alarm) - central log analysis & Metric Filters + Alarms (turn events into alerts)
- S3 Bucket - long-term storage for CloudTrail logs
- Simple Notification Service (SNS) - email notifications
- E-mail


This project implements a monitoring and alerting pipeline that detects when my secret in AWS Secrets Manager is accessed (via console or CLI) I will be notified via Amazon SNS. 
This solution has many components, CloudTrail, CloudWatch, alarms, SNS & S3, Secrets Manager.
It also includes a second approach direct CloudTrail SNS delivery and compares both methods,  

If i took away CloudWatch & alarms from the solution, what will happen?
I will configure direct CloudTrail notifications & compare it against using CloudWatch alarms 
In this solution, I will configure direct SNS notification form CloudTrail, Compare notification methods between CloudWatch & CloudTrail


<img width="2172" height="1265" alt="image" src="https://github.com/user-attachments/assets/7b9fbfa2-0f71-4452-b8a4-45d22096194d" />


# Creating The Secret

AWS secrets manager is a security service for storing secrets i.e. database credentials, account IDs, API Keys & anything that is classed as sensitive information that could cause damage if access to gained without authority.
I created a secret called lifehack in Secrets Manager. this secret is a string that contains a phrase by Charles Bukowski  "Don’t Try"

