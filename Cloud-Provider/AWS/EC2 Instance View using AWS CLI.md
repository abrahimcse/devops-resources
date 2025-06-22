# 🖥️ EC2 Instance Details Viewer using AWS CLI

A handy Bash script to list **EC2 instances** with their **Name tag**, **Private/Public IP**, **Instance Type**, and **Status** in a clean table format using AWS CLI.

---

## 📦 Prerequisites

Before running the script, ensure the following tools are installed and configured on your machine:

### ✅ AWS CLI Installation

[AWS Official Documents](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Install AWS CLI (if not already installed):

```bash
sudo apt install awscli        # Ubuntu/Debian
# OR
sudo yum install awscli        # RHEL/CentOS
# OR
brew install awscli            # macOS
```

### ✅ AWS CLI Configuration

Configure your AWS credentials:

```bash
aws configure
```

You’ll be prompted to enter:

```
AWS Access Key ID [None]: YOUR_ACCESS_KEY
AWS Secret Access Key [None]: YOUR_SECRET_KEY
Default region name [None]: us-east-1
Default output format [None]: json
```

🔒 **Credentials File Location:**

Your credentials will be saved at:

```bash
~/.aws/credentials
cat ~/.aws/credentials
```

Example content:

```ini
[default]
aws_access_key_id = AKIAxxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 🛠️ Setup Instructions

### 1. Create the script

Create a new file:

```bash
vim ec2.sh
```

Paste the following content:

```bash
#!/bin/bash

# This script fetches and displays details of EC2 instances, 
# including their names, private and public IP addresses, instance types, and status in a formatted table using AWS CLI.

if ! command -v aws &>/dev/null; then
    echo "AWS CLI is not installed. Please install it and configure it."
    exit 1
fi

aws ec2 describe-instances \
    --query 'Reservations[].Instances[].[
        Tags[?Key==\`Name\`].Value | [0] || \`(no name)\`, 
        PrivateIpAddress || \`-\`, 
        PublicIpAddress || \`-\`, 
        InstanceType, 
        State.Name
    ]' \
    --output text |
    tr '\t' '|' |
    awk -F '|' 'BEGIN { 
        print "+--------------------------------+----------------+----------------+----------------+------------+";
        print "| Instance Name                  | Private IP     | Public IP      | Instance Type  | Status     |";
        print "+--------------------------------+----------------+----------------+----------------+------------+";
     }
     {
        printf("| %-30s | %-14s | %-14s | %-14s | %-10s |\n", $1, $2, $3, $4, $5);
     }
     END {
        print "+--------------------------------+----------------+----------------+----------------+------------+";
     }'
```

### 2. Make the script executable

```bash
chmod +x ec2.sh
```

### 3. Run the script

```bash
./ec2.sh
```

---

## ✅ Example Output

```
+--------------------------------+----------------+----------------+----------------+------------+
| Instance Name                  | Private IP     | Public IP      | Instance Type  | Status     |
+--------------------------------+----------------+----------------+----------------+------------+
| stg-k8s-master                 | 10.10.8.159    | -              | t3a.medium     | running    |
| stg-k8s-worker-1               | 10.10.8.211    | -              | t3a.xlarge     | running    |
| stg-kafka                      | 10.10.8.176    | -              | t3a.large      | running    |
+--------------------------------+----------------+----------------+----------------+------------+
```
