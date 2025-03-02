# Staging Environment Setup

## 1. VPC Creation
### Step-by-Step Guide:
1. Navigate to AWS Management Console → **VPC** service.
2. Click on **Create VPC**.
3. Set **VPC Name**: `hsms-stg-vpc`.
4. Choose IPv4 CIDR Block as per requirement (e.g., `10.0.0.0/16`).
5. Click **Create VPC**.

## 2. Subnet Creation
### Step-by-Step Guide:
1. Go to **Subnets** under **VPC**.
2. Click **Create Subnet**.
3. Select **VPC**: `hsms-stg-vpc`.
4. Define Subnets:
   - **Public Subnet**: `hsms-stg-subnet-public1-ap-southeast-1a` (e.g., `10.0.1.0/24`)
   - **Private Subnet**: `hsms-stg-subnet-private1-ap-southeast-1a` (e.g., `10.0.2.0/24`)
5. Click **Create Subnet**.

## 3. Route Tables
### Step-by-Step Guide:
1. Go to **Route Tables**.
2. Click **Create Route Table**.
3. Define Route Tables:
   - **Public Route Table**: `hsms-stg-rtb-public` → Associate with **public subnet**.
   - **Private Route Table**: `hsms-stg-rtb-private1-ap-southeast-1a` → Associate with **private subnet**.
   - **Private Route Table (2nd AZ)**: `hsms-stg-rtb-private2-ap-southeast-1b`.
4. Click **Create** and edit routes as needed.

## 4. Internet Gateway
### Step-by-Step Guide:
1. Go to **Internet Gateways**.
2. Click **Create Internet Gateway**.
3. Name it: `hsms-stg-igw`.
4. Attach it to **hsms-stg-vpc**.

## 5. NAT Gateway
### Step-by-Step Guide:
1. Go to **NAT Gateways**.
2. Click **Create NAT Gateway**.
3. Select **public subnet**.
4. Allocate **Elastic IP**.
5. Click **Create NAT Gateway**.

## 6. Elastic IPs
### Step-by-Step Guide:
1. Go to **Elastic IPs**.
2. Click **Allocate New Address**.
3. Allocate two EIPs:
   - `haproxy-lb-eip`
   - `hsms-stg-eip-ap-southeast-1a-nat-gateway`

## 7. Resource Mapping
```
hsms-stg-vpc
|
├── hsms-stg-subnet-public1-ap-southeast-1a  → hsms-stg-rtb-public → hsms-stg-igw
|
├── hsms-stg-subnet-private1-ap-southeast-1a → hsms-stg-rtb-private1-ap-southeast-1a → hsms-stg-nat-public1-ap-southeast-1a
|
└── hsms-stg-rtb-private2-ap-southeast-1b → hsms-stg-nat-public1-ap-southeast-1a
```

## 8. Security Groups
### Public Load Balancer Security Group (`public-lb-sg`)
#### Step-by-Step Guide:
1. Go to **Security Groups**.
2. Click **Create Security Group**.
3. Name: `public-lb-sg`.
4. Inbound Rules:
   - HTTPS (443) - Allow
   - HTTP (80) - Allow
   - SSH (22) - Allow
5. Outbound Rules:
   - All Traffic - Allow
6. Click **Create Security Group**.

### Internal Security Group (`stg-allow-internal-sg`)
#### Step-by-Step Guide:
1. Click **Create Security Group**.
2. Name: `stg-allow-internal-sg`.
3. Inbound Rules:
   - All Traffic - Allow
4. Outbound Rules:
   - All Traffic - Allow
5. Click **Create Security Group**.

---
This guide ensures a structured approach to setting up the staging environment in AWS. 