# Step-by-Step Implementation Guide

This document provides a full walkthrough of how the AWS security monitoring
system was built and tested, with screenshots for verification.

## Stage 1 — Set Up Secrets & Logging

### Step 1: Create a Secret in AWS Secrets Manager

- Created a secret in secret manager `LifeHack`. This is the secret I want to monitor access to.
- Stored a test string value
- Enabled encryption using AWS KM

- [Create secret in Secrets Manager]

 <img width="1250" height="572" alt="Screenshot 2025-12-31 040705" src="https://github.com/user-attachments/assets/9c554a97-49da-4b25-bbdf-818133343784" />

In this step I set up a secret in Secret Manager. This is what i want to protect & monitor access to across this project.
