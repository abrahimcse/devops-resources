# 🔐 AWS Cross-Account Access Setup Guide

This document provides a complete step-by-step guide for setting up **Cross-Account Access in AWS using IAM Roles**. This allows users or services in one AWS account to access resources in another AWS account securely.

---

## 📌 Use Case

- **Account A (Source / Dev Account)**  
  Account ID: `111111111111`

- **Account B (Target / Prod Account)**  
  Account ID: `222222222222`

**Goal:** Allow IAM users or services in **Account A** to access resources (e.g., S3, EC2) in **Account B**.

---

## ✅ Steps Overview

1. [Create IAM Role in Account B](#1-create-an-iam-role-in-account-b-target-account)
2. [Edit Trust Policy in Account B](#2-verifytrust-policy-in-account-b)
3. [Allow AssumeRole in Account A](#3-grant-access-in-account-a-source-account)
4. [Assume Role Using CLI or SDK](#4-assume-the-role-programmatically-or-via-cli)
5. [Optional: Automation (Terraform/GitHub Actions)](#5-optional-automation)

---

## 1. Create an IAM Role in Account B (Target Account)

1. Log in to **Account B (Prod)**.
2. Go to **IAM > Roles > Create Role**.
3. Choose **Another AWS Account**.
4. Enter **Account A ID** (`111111111111`).
5. (Optional) Add **External ID** if needed.
6. Click **Next**, then attach required policies (e.g., `AmazonS3FullAccess` or a custom policy).
7. Name the role: e.g., `CrossAccountAccessFromDev`.
8. Click **Create Role**.

---

## 2. Verify/Edit Trust Policy in Account B

Go to the created role in Account B > **Trust Relationships > Edit Trust Policy**, and use:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111111111111:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

> ✅ You can replace `root` with a specific IAM role or user if preferred:
> `arn:aws:iam::111111111111:role/DevOpsRole`

---

## 3. Grant Access in Account A (Source Account)

### ➤ Create IAM Policy to Allow Role Assumption:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::222222222222:role/CrossAccountAccessFromDev"
    }
  ]
}
```

### ➤ Attach This Policy

- Attach it to the IAM user, group, or role in **Account A** who needs access.

---

## 4. Assume the Role Programmatically or via CLI

### ➤ Using AWS CLI

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::222222222222:role/CrossAccountAccessFromDev \
  --role-session-name dev-session
```

### ➤ Output Will Contain:

```json
{
  "Credentials": {
    "AccessKeyId": "ASIA...",
    "SecretAccessKey": "abc123...",
    "SessionToken": "FwoGZXIvYXdz...",
    "Expiration": "2025-05-25T18:30:00Z"
  }
}
```

### ➤ Export These Credentials:

```bash
export AWS_ACCESS_KEY_ID=ASIA...
export AWS_SECRET_ACCESS_KEY=abc123...
export AWS_SESSION_TOKEN=FwoGZXIvYXdz...
```

> Now you can run commands like: `aws s3 ls` or `aws ec2 describe-instances` in **Account B**.

---

## 5. Optional: Automation

### ➤ Terraform Example:

```hcl
provider "aws" {
  alias  = "prod"
  region = "us-east-1"

  assume_role {
    role_arn = "arn:aws:iam::222222222222:role/CrossAccountAccessFromDev"
  }
}
```

---

## 🔐 Security Best Practices

- ✅ Use **least privilege**: Only grant required permissions.
- ✅ Use **External ID** for third-party integrations.
- ✅ Enable **CloudTrail** to monitor role assumptions.
- ✅ Rotate and audit roles regularly.
- ✅ Avoid `*` in Resource or Action unless absolutely necessary.

---

## 📊 Summary Table

| Task                         | Account A (Dev) ✅ | Account B (Prod) ✅ |
|------------------------------|-------------------|---------------------|
| Create IAM Policy            | ✅                | ❌                  |
| Create IAM Role              | ❌                | ✅                  |
| Attach Permissions           | ❌                | ✅                  |
| Edit Trust Relationship      | ❌                | ✅                  |
| Assume Role via CLI/SDK      | ✅                | ❌                  |

---

## 🧾 References

- [AWS Docs – Cross-Account Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)
- [AWS CLI – assume-role](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html)
- [Best Practices for IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)