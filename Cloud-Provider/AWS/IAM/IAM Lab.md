# AWS IAM Lab Guide

This document contains step-by-step instructions for AWS IAM labs, including creating IAM users, groups, roles, and policies.

---

## Lab 1: Create IAM User

**Objective:** Create an IAM user with programmatic and console access.

1. Login to [AWS Console](https://aws.amazon.com/console/).
2. Go to **IAM** service.
3. Click on **Users** → **Add users**
4. Enter:
   - **User name**: `dev-user`
   - Check both:
     - ✅ Programmatic access
     - ✅ AWS Management Console access
5. Set a password (Auto-generated or custom).
6. Click **Next: Permissions**

---

## Lab 2: Create Group and Assign to User

**Objective:** Create a group with predefined permissions and assign the user to this group.

1. On **Set permissions** page:
   - Choose: **Add user to group**
   - Click **Create group**
   - Group name: `Developers`
   - Attach policy: ✅ `AmazonEC2ReadOnlyAccess`
   - Click **Create group**
2. User is now in `Developers` group with EC2 read-only access.
3. Click **Next → Create user**
4. ✅ Note down user login link and credentials

---

## Lab 3: Create Custom IAM Policy

**Objective:** Create a policy that allows only listing all S3 buckets.

1. Go to **Policies** → Click **Create policy**
2. Choose **JSON** tab, paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    }
  ]
}
```

3. Click **Next** → Name it `S3ListBucketsOnly`
4. Click **Create policy**

---

## Lab 4: Attach Custom Policy to Group

1. Go to **Groups** → click on `Developers`
2. Go to **Permissions** tab → Click **Add permissions**
3. Choose: **Attach policies**
4. Search and select `S3ListBucketsOnly`
5. Click **Attach policy**

---

## Lab 5: Create IAM Role for EC2

**Objective:** Create a role to allow EC2 to access S3.

1. Go to **Roles** → **Create role**
2. Select **Trusted entity**: `AWS service`
3. Use case: ✅ `EC2`
4. Click **Next**
5. Attach permissions: ✅ `AmazonS3ReadOnlyAccess`
6. Click **Next** → Role name: `EC2_S3_ReadOnly`
7. Create role

---

## Lab 6: Launch EC2 Instance with Role

1. Go to **EC2** → **Launch instance**
2. Choose Amazon Linux or Ubuntu
3. In **IAM Role**, choose: `EC2_S3_ReadOnly`
4. Launch instance

---

## Lab 7: Test IAM Role from EC2

1. SSH into the EC2 instance
2. Run:

```bash
aws s3 ls
```

> If AWS CLI is not installed, install and configure it first.

---

## Lab 8: Create Inline Policy

**Objective:** Add a specific policy directly inside a user or group.

1. Go to **Groups** → Select `Developers`
2. Go to **Permissions** tab → Click **Add inline policy**
3. Choose **S3** service
4. Choose action: `GetObject`
5. Choose a specific bucket or object
6. Review and name it: `InlineS3ObjectReader`
7. Create policy

---

## Lab 9: Enable MFA for IAM User

1. Login with `dev-user` credentials
2. Go to **IAM** → **Users**
3. Click on `dev-user` → **Security credentials**
4. Enable **MFA** → Choose **Virtual MFA device**
5. Use Google Authenticator to scan QR code and complete setup

---

## Summary Table

| Task                          | Tool       | Name/Value              |
|-------------------------------|------------|--------------------------|
| Create User                   | Console    | dev-user                |
| Create Group                  | Console    | Developers              |
| Attach Policy to Group        | Console    | AmazonEC2ReadOnlyAccess |
| Create Custom Policy          | Console    | S3ListBucketsOnly       |
| Attach Custom Policy          | Console    | To Developers group     |
| Create IAM Role for EC2       | Console    | EC2_S3_ReadOnly         |
| Launch EC2 with Role          | Console    | EC2 Instance            |
| Create Inline Policy          | Console    | InlineS3ObjectReader    |
| Enable MFA                    | Console    | Virtual MFA             |

---

## Optional: AWS CLI Instructions

If you want the same steps using AWS CLI, let me know!
