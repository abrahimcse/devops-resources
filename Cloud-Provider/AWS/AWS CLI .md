
# AWS CLI Setup on Ubuntu

This guide explains how to install, configure, and use the AWS Command Line Interface (CLI) on Ubuntu.

---

## 📌 What is AWS CLI?

AWS CLI (Command Line Interface) is a tool that enables you to manage AWS services from the command line using terminal commands. It interacts with AWS services through their APIs.

---

## ✅ Why Use AWS CLI?

- Automate AWS service management
- Access AWS resources using scripts
- Manage EC2, S3, IAM, RDS, etc.
- Faster than using the AWS Console
- Ideal for DevOps and system administrators

## 🔶 Why Install AWS CLI Separately?

Think of your Ubuntu terminal like a **mobile phone** - it can make calls by default, but to use **WhatsApp**, you need to install it separately.

🟢 **Ubuntu Terminal** = Basic phone (can run simple commands)  
🔵 **AWS CLI** = WhatsApp (needs special installation to talk to AWS servers)  

📌 Without installing AWS CLI, your terminal won't understand commands like:
```bash
aws s3 ls  # This won't work!
```
---

## 🖥️ Prerequisites

Before installing AWS CLI, ensure your system has:

- Ubuntu 18.04 or later
- `curl` and `unzip` installed

```bash
sudo apt update
sudo apt install curl unzip -y
```

---

## 📥 Installation Steps

### 1. Download the AWS CLI Installer

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

### 2. Unzip the Installer

```bash
unzip awscliv2.zip
```

### 3. Run the Installer

```bash
sudo ./aws/install
```

### 4. Verify the Installation

```bash
aws --version
```

Expected output:

```text
aws-cli/2.x.x Python/3.x.x Linux/...
```

---

## 🔐 Configure AWS CLI

You need AWS credentials to use the CLI. Run:

```bash
aws configure
```

Provide the following when prompted:

- AWS Access Key ID
- AWS Secret Access Key
- Default region (e.g. `us-east-1`)
- Output format (`json` recommended)

---

## ⚙️ Basic Usage Examples

### List S3 Buckets

```bash
aws s3 ls
```

### List EC2 Instances

```bash
aws ec2 describe-instances
```

### Upload File to S3

```bash
aws s3 cp myfile.txt s3://your-bucket-name/
```

---

## 🧾 Uninstall AWS CLI (if needed)

```bash
sudo /usr/local/bin/aws uninstall
```

---

## 📚 Useful Links

- [Official AWS CLI Documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)
