# AWS VPC Setup Guide



AWS **VPC (Virtual Private Cloud)** is a logically isolated network within AWS where you can launch and manage cloud resources securely. It acts like a private data center in the cloud.

###  🔹Key Features  
- **Custom IP Range**: Define your own CIDR block (e.g., `10.0.0.0/16`).  
- **Subnets**: Divide the VPC into **public and private subnets** for better security.  
- **Internet Gateway (IGW)**: Allows public internet access for public subnets.  
- **Routing**: Custom **Route Tables** determine how traffic flows inside the VPC.  
- **Security**: Uses **Security Groups** and **Network ACLs** to control traffic.  
---
## Table of Contents
### 🔹Create a VPC and Associated Resources

**Step 1:** Create a VPC

**Step 2:** Create an Internet Gateway (IGW)

**Step 3:** Create Subnets

**Step 4:** Create Route Tables

**Step 5:** Associate Subnets with Route Tables

**Step 6:** Add Routes to the Route Table

### 🔹 Delete a VPC and Associated Resources

**Step 1:** Delete EC2 Instances or Other Resources

**Step 2:** Delete NAT Gateways (if applicable)

**Step 3:** Delete Elastic Network Interfaces (ENIs)

**Step 4:** Delete Subnets

**Step 5:** Delete Route Tables

**Step 6:** Delete Internet Gateway

**Step 7:** Delete VPC

---

## 🚀 Order of VPC Creation  

### **1️⃣ Create a VPC**  
What it is: A Virtual Private Cloud (VPC) is your private network in AWS.  

**Steps:**  
1. Navigate to the **VPC Dashboard** in AWS.  
2. Click **Create VPC**.  
3. Enter a **Name tag** (e.g., `abrahim-VPC`).  
4. Specify an **IPv4 CIDR block** (e.g., `10.0.0.0/16`).  
5. Leave the other settings as default and **click Create VPC**.  

---

### **2️⃣ Create an Internet Gateway (IGW)**  
What it is: An Internet Gateway allows communication between your VPC and the internet.  

**Steps:**  
1. Go to the **Internet Gateways** section in the VPC Dashboard.  
2. Click **Create Internet Gateway**.  
3. Enter a **Name tag** (e.g., `abrahim-IGW`).  
4. Click **Create Internet Gateway**.  
5. After creation, **attach it to the VPC**:  
   - Select the IGW  
   - Click **Actions > Attach to VPC**  
   - Choose the **VPC you created earlier (e.g., MyVPC)** and **click Attach**  

---

### **3️⃣ Create Subnets**  
What it is: Subnets are segments of your VPC’s IP address range where you can place resources.  

**Steps to create a Public Subnet:**  
1. Navigate to **Subnets** in the VPC Dashboard.  
2. Click **Create Subnet**.  
3. Select the **VPC you created earlier** (e.g., `abrahim-VPC`).  
4. Enter a **Subnet Name** (e.g., `PublicSubnet`).  
5. Select an **Availability Zone** (e.g., `us-east-1a`).  
6. Enter an **IPv4 CIDR block** (e.g., `10.0.1.0/24`).  
7. Click **Create Subnet**.  

**Steps to create a Private Subnet:**  
Repeat the above process but:  
- Use a **different CIDR block** (e.g., `10.0.2.0/24`).  
- Name it **PrivateSubnet**.  

---

### **4️⃣ Create Route Tables**  
What it is: A **route table** controls how traffic is routed within your VPC.  

**Steps to create a Public Route Table:**  
1. Go to **Route Tables** in the VPC Dashboard.  
2. Click **Create Route Table**.  
3. Enter a **Name tag** (e.g., `PublicRouteTable`).  
4. Select the **VPC** (e.g., `abrahim-VPC`).  
5. Click **Create Route Table**.  

**Steps to create a Private Route Table:**  
Repeat the above process but name it **PrivateRouteTable**.  

---

### **5️⃣ Associate Subnets with Route Tables**  
**Steps to associate Public Subnet with Public Route Table:**  
1. Select the **Public Route Table**.  
2. Go to the **Subnet Associations** tab.  
3. Click **Edit Subnet Associations**.  
4. Select the **Public Subnet (e.g., PublicSubnet)** and click **Save**.  

**Steps to associate Private Subnet with Private Route Table:**  
Repeat the above steps using the **Private Route Table and Private Subnet**.  

---

### **6️⃣ Add Routes to the Route Table**  
**Steps to add an Internet Route for Public Route Table:**  
1. Select the **Public Route Table**.  
2. Go to the **Routes** tab.  
3. Click **Edit Routes**.  
4. Add a route:  
   - **Destination**: `0.0.0.0/0` (all traffic).  
   - **Target**: Select the **Internet Gateway** (`abrahim-IGW`).  
5. Click **Save Routes**.  

For the **Private Route Table**, you typically **do not** add an internet route unless you want private resources to access the internet (e.g., via a NAT Gateway).  

---

## 🔄 Order of VPC Deletion  

### **🗑️ Step 1: Delete EC2 Instances**  
1. Go to the **EC2 Dashboard**.  
2. Select the **instances** you want to delete.  
3. Click **Instance State > Terminate Instance**.  
4. Confirm the termination.  

---

### **🗑️ Step 2: Delete NAT Gateways (if applicable)**  
1. Go to the **VPC Dashboard**.  
2. Navigate to **NAT Gateways**.  
3. Select the **NAT Gateway** to delete.  
4. Click **Actions > Delete NAT Gateway**.  
5. Confirm the deletion.  

---

### **🗑️ Step 3: Delete Elastic Network Interfaces (ENIs)**  
1. Go to the **EC2 Dashboard**.  
2. Navigate to **Network Interfaces**.  
3. Select the **ENIs associated with the VPC**.  
4. Click **Actions > Delete Network Interface**.  
5. Confirm the deletion.  

---

### **🗑️ Step 4: Delete Subnets**  
1. Go to the **VPC Dashboard**.  
2. Navigate to **Subnets**.  
3. Select the **subnets you want to delete**.  
4. Click **Actions > Delete Subnet**.  
5. Confirm the deletion.  

---

### **🗑️ Step 5: Delete Route Tables**  
1. Go to the **VPC Dashboard**.  
2. Navigate to **Route Tables**.  
3. Select the **custom route tables (not the main/default route table)**.  
4. Click **Actions > Delete Route Table**.  
5. Confirm the deletion.  

---

### **🗑️ Step 6: Delete Internet Gateway**  
1. Go to the **VPC Dashboard**.  
2. Navigate to **Internet Gateways**.  
3. Select the **Internet Gateway**.  
4. Click **Actions > Detach from VPC**.  
5. Confirm the detachment.  
6. Click **Actions > Delete Internet Gateway**.  
7. Confirm the deletion.  

---

### **🗑️ Step 7: Delete VPC**  
1. Go to the **VPC Dashboard**.  
2. Navigate to **Your VPCs**.  
3. Select the **VPC you want to delete**.  
4. Click **Actions > Delete VPC**.  
5. Confirm the deletion.  

---

## 📖 References  

- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/latest/reference/ec2/)  
- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)  

---

## 📢 Author  

**Abdur Rahim**   
[LinkedIn](https://www.linkedin.com/in/abrahimcse/) | [GitHub](https://github.com/your-github-profile)  

---


