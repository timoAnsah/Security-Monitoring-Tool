# Step-by-Step Implementation Guide

This document provides a full walkthrough of how the AWS security monitoring
system was built and tested, with screenshots for verification.

## Step 1: Create a Secret in AWS Secrets Manager
AWS secrets manager is a security service for storing secrets i.e. database credentials, account IDs, API Keys 
& anything that is classed as sensitive information that could cause damage  if access to gained without authority.

- Created a secret in secret manager `LifeHack`. This is the secret I want to monitor access to.
- In the key/value tab, the key is `The Secret is`, the value is `Don't Try` These helps database go through all the secrets stored & retrieve the right one for you.
- Enabled encryption using AWS KMS key `aws/secretsmanager`. it scrambles the secret so only authorized users with the right key can descramble it.

 <img width="1227" height="552" alt="Screenshot 2025-12-31 040705" src="https://github.com/user-attachments/assets/36a26611-aa92-4d7a-babd-6c9fca8f573b" />

 <img width="1250" height="655" alt="Screenshot 2025-12-13 005752" src="https://github.com/user-attachments/assets/609042e1-3ce2-47bb-a042-80d6d9b33bf6" />


## Step 2: Configure CloudTrail to track access to my secret.

CloudTrail is a monitoring service used to track events & activities in our AWS accounts. this continuous recording is valuable for security like spotting unusual activity, troubleshooting - 
what actually happened behind the scene & meeting compliance requirements like proving to a client you encrypt data. These logs are helpful for security, detecting suspicious activity & for compliance i.e. 

- create a CloudTrail `secrets-manager-trail` to track when the secret is accessed.
- Configure the trail to store logs in the S3 Bucket, useful for long term storage. CloudTrail logs are deleted after 90 days
- I will only enable SSE-KMS encrption in a production environment
- I selected management events because accessing a secret falls into that category.
- Select `Write & Read` API activity. Here, I ticked both, acccessing a secret is considered a `Write` it invovles creating, deleting & updating a resource  
  `Read` involves accessing, reading & opening a resouce. 


<img width="3007" height="1600" alt="Screenshot 2025-12-13 012226" src="https://github.com/user-attachments/assets/489ba2d9-f367-40ab-8ccf-ce92bb022d00" />

<img width="3360" height="1532" alt="Screenshot 2025-12-13 013700" src="https://github.com/user-attachments/assets/fe2ab2b9-7cc8-400a-8bb6-27921e155374" />

<img width="1247" height="623" alt="Screenshot 2025-12-31 050642" src="https://github.com/user-attachments/assets/2ee62521-d39d-48a3-82d5-6775d49120ca" />

<img width="1250" height="1172" alt="image" src="https://github.com/user-attachments/assets/a91a4403-2d0e-4768-8ddf-277014e8f975" />




Verifying the set-up above? Can CloudTrail detect when I access my Secret in Secret's Manager?






## Retrieve Secret via the Console

`Retrieve Secret Value` opens the actual content of the secret. This triggers `GetSecretValue` API call to AWS. Its this specific API call that we want to monitor with CloudTrail, it represents someone reading your secret.

<img width="1230" height="510" alt="Screenshot 2025-12-31 052442" src="https://github.com/user-attachments/assets/e3eb851d-47b1-41ae-aaa5-b7849e065370" />

<img width="1210" height="187" alt="Screenshot 2025-12-31 052717" src="https://github.com/user-attachments/assets/e837fd01-bc7a-425f-ac71-2c020a5cbe9e" />

## Retrieve Secret via the CLI

- In the Cloudshell Terminal I run this AWS command `aws secretsmanager get-secret-value --secret-id "LifeHack" --region eu-west-2`

<img width="955" height="758" alt="Screenshot 2025-12-14 172115" src="https://github.com/user-attachments/assets/9c801d27-0dbd-47a0-8725-f1f235dba3cb" />








## Verify CloudTrail recorded logs

- to Analyse the events, I selected `Event Source` & `secrets-manager`.
- Filtering with this enabled me to spot the secrets & reduce the time spent going through unrelated logs.
- Evidence of CloudTrail tracking access to my secret `GetSecretValue`

<img width="1461" height="580" alt="Screenshot 2025-12-31 054337" src="https://github.com/user-attachments/assets/cf3224a5-8f55-43da-b864-d4f8015c0655" />




## Step 3: Enable CloudWatch Logs for CloudTrail & Define a Metric for the alarm

Its a monitoring service that brings together logs from other AWS Services for visibility, troubleshooting & deeper analysis & insight on events happening in my account.
This insight helps create metrics on how many times an event happens.

- `Enable` checkbox for CloudWatch logs
- select existing CloudTrail log loggroup as delivery endpoint for the logs.
- CloudWatch needs an IAM Role to work, to ensure CloudWatch needs access to CloudTrail
- IAM Role is a way to control access our resources has to other things. A role is a great way to contain permissions to exactly what it needs which is the principle of least privileges
- Under log Management select log group & create a metric filter.



<img width="1238" height="402" alt="Screenshot 2025-12-31 060242" src="https://github.com/user-attachments/assets/4eab5d7f-4fda-4569-96f1-e3a7f8ea7a46" />


<img width="1232" height="458" alt="Screenshot 2025-12-31 060646" src="https://github.com/user-attachments/assets/e5ba0e90-d1f8-42cb-a6a1-731cfbabcedc" />



In the CloudWatch console I’m going to verify that CloudTrail is really passing the logs to a new log group.

- In CloudWatch, log management contains the log streams. Each log is one event & it matches up to the log in CloudTrail. 
  I set up alerts for when I access my secret using advanced filtering CloudWatch logs are powerful filtering tools that enables me to focus exactly on the events i care about. 

- CloudTrail events history is useful for quickly reading management events that happened in the last 90 days while CloudWatch logs are better for combining and analysing logs from different sources & accessing logs for longer than 90 days & advanced filtering. 
  CloudWatch Logs retrieve logs from the S3 Bucket that stores CloudTrail Logs.

- This approach allows CloudWatch Logs to access the logs without requiring direct integration with CloudTrail which simplifies the set up and reduces potential for dependencies between the two services. This ensures CloudWatch Logs configuration remains consistent even if you change CloudTrail settings such as the S3 Bucket Location


<img width="2560" height="430" alt="Screenshot 2025-12-31 061959" src="https://github.com/user-attachments/assets/c1ee7385-02a6-4d20-b72d-f8b2cca94566" />


<img width="2342" height="740" alt="Screenshot 2025-12-31 063331" src="https://github.com/user-attachments/assets/1bc8db4e-1b6a-43f4-875d-d0560142c270" />

<img width="2347" height="887" alt="Screenshot 2025-12-31 063629" src="https://github.com/user-attachments/assets/9be53126-9a3c-4e24-97d2-351e2986f455" />

