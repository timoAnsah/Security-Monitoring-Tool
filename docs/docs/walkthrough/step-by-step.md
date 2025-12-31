# Step-by-Step Implementation Guide

This document provides a full walkthrough of how the AWS security monitoring
system was built and tested, with screenshots for verification.

## Stage 1 — Set Up Secrets & Logging

### Step 1: Create a Secret in AWS Secrets Manager

- Created a secret in secret manager `LifeHack`. This is the secret I want to monitor access to.
- In the key/value tab, the key is `The Secret is`, the value is `Don't Try` These helps database go through all the secrets stored & retrieve the right one for you.
- Enabled encryption using AWS KMS key `aws/secretsmanager`. it scrambles the secret so only authorized users with the right key can descramble it.

- [Create secret in Secrets Manager]

 <img width="1227" height="552" alt="Screenshot 2025-12-31 040705" src="https://github.com/user-attachments/assets/36a26611-aa92-4d7a-babd-6c9fca8f573b" />

 <img width="1637" height="786" alt="Screenshot 2025-12-13 005752" src="https://github.com/user-attachments/assets/75a7c792-c032-44da-a1f3-47895f107467" />


In this step I set up a secret in Secret Manager. This is what i want to protect & monitor access to across this project.
