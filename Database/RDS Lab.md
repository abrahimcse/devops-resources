
# 🧪 AWS RDS MySQL Lab Guide (Easy Steps)

## 🔧 Step 1: Login to AWS
- Go to [https://console.aws.amazon.com](https://console.aws.amazon.com)
- Login with your AWS credentials.

## 📦 Step 2: Open RDS Service
- In the **Search bar**, type `RDS` and click **RDS**.
- You'll be redirected to the RDS Dashboard.

## 🏗️ Step 3: Create Database
- Click **"Create database"**.
- Select **Standard Create**.

## 🛠️ Step 4: Choose Database Engine
- Choose **MySQL**.

## 📋 Step 5: Choose a Template
- Select one:
  - **Free Tier** (for learning – recommended)
  - Dev/Test
  - Production

## 🔑 Step 6: Settings
- DB Instance Identifier: `my-mysql-db`
- Master Username: `admin`
- Master Password: `MySecurePass123` (Note it down)
- Confirm Password

## 💾 Step 7: DB Instance Class
- Select Free Tier option: `db.t3.micro` (or `db.t2.micro`)
- Storage: 20 GB (default)
- Enable **Storage auto-scaling** (optional)

## 🌐 Step 8: Connectivity
- Virtual Private Cloud (VPC): Default
- Subnet group: default
- Public access: **Yes** (for testing only – never for production)
- VPC security group: Choose **Create new** or **Select existing**
  - ➡️ Allow port **3306** (MySQL default port)

## 🔒 Step 9: Additional Configuration
- Initial database name: `mydb`
- Enable automated backups: Optional
- Backup retention: 1 day
- Maintenance window: Default

## 🚀 Step 10: Create Database
- Click **Create database** at the bottom.
- Wait 5–10 minutes for the instance to become **"Available"**.

## 🔗 Step 11: Connect to Your MySQL RDS

### ✅ Option A: From MySQL Workbench
1. Open MySQL Workbench.
2. Click on **+** to create a new connection.
3. Connection Name: `AWS-RDS`
4. Hostname: **RDS endpoint** (from RDS Dashboard)
5. Port: `3306`
6. Username: `admin`
7. Password: `MySecurePass123`
8. Click **Test Connection** → **OK**

### ✅ Option B: From Command Line
```bash
mysql -h <your-endpoint> -P 3306 -u admin -p
```

## ✅ Step 12: Test Commands (Basic)
```sql
SHOW DATABASES;
USE mydb;
CREATE TABLE users (id INT, name VARCHAR(100));
INSERT INTO users VALUES (1, 'Rahim');
SELECT * FROM users;
```

## 📌 Clean Up (After Practice)
To avoid charges:
- Go to RDS Dashboard.
- Select your DB instance.
- Click **Actions > Delete**.
- Confirm with snapshot (or skip).
- Click **Delete**.

## 🎓 Summary

| Task                 | Done? |
|----------------------|-------|
| Create RDS MySQL DB  | ✅    |
| Connect from client  | ✅    |
| Run SQL commands     | ✅    |
| Delete after use     | ✅    |