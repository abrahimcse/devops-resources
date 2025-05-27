
# AWS RDS (Relational Database Service) – Full Guide

## 1. What is AWS RDS?
AWS Relational Database Service (RDS) is a managed database service that lets you run databases in the cloud without handling servers, backups, or updates. AWS takes care of setup, maintenance, scaling, and security.

---

## 2. Types of Databases Supported by RDS
- **Amazon Aurora** – AWS's own high-performance DB
- **MySQL** – Popular open-source
- **PostgreSQL**  – dvanced open-source
- **MariaDB**  – MySQL fork, fully open-source
- **Oracle**  – Enterprise-grade
- **Microsoft SQL Server**  – For Windows-based apps

---

## 3. RDS Templates (When Creating an RDS Instance)
AWS Console gives you 3 templates:
- **Production** – Best for live apps, high performance, high availability (Multi-AZ).
- **Dev/Test** – For development and testing. Lower cost.
- **Free Tier** – For new users (750 hours/month for db.t3.micro instance, only for MySQL, PostgreSQL, or MariaDB).

---

## 4. DB Storage in RDS

### Storage Types:
- **General Purpose (SSD)**: Balanced performance for most workloads.
- **Provisioned IOPS (SSD)**: High performance for heavy database workloads.
- **Magnetic (deprecated)**: Old, slow, and not recommended.

### Storage Auto-Scaling:
- Automatically increases storage if needed (configurable).

---

## 5. Backups in RDS

### Automated Backups:
- Enabled by default.
- Daily full backups + transaction logs (allows point-in-time recovery).
- Retention period: 1 to 35 days.

### Manual Snapshots:
- User-triggered backups stored until deleted.
- Useful for long-term retention.

---

## 6. Maintenance in RDS

- **Software Patching**: AWS automatically applies patches (can be scheduled).
- **Minor Version Upgrades**: Can be automatic or manual.
- **Major Version Upgrades**: Requires manual approval (may cause downtime).
- **Maintenance Window**: Choose a time (e.g., late night) to minimize disruption.

---

## 7. Billing & Cost

### Pricing Factors:
- **Instance Type** (e.g., db.t3.small, db.m5.large)
- **Storage** (GB/month)
- **Backup Storage** (Beyond free allowance)
- **Data Transfer** (Outbound from AWS)
- **Multi-AZ Deployment** (Extra cost for high availability)

### Free Tier:
- Limited free usage for 12 months (e.g., 750 hrs/month of db.t2.micro)

---

## 8. Types of RDS Databases

- **MySQL** – Popular open-source database.
- **PostgreSQL** – Advanced open-source DB with extra features.
- **MariaDB** – Fork of MySQL, fully open-source.
- **Oracle** – Enterprise-grade commercial DB.
- **SQL Server** – Microsoft’s relational DB.
- **Amazon Aurora** – AWS-optimized MySQL/PostgreSQL (faster & more scalable).

---

## 9. Key Features
- ✔ Automated Backups & Restore
- ✔ Multi-AZ for High Availability (failover support)
- ✔ Read Replicas (Improve read performance)
- ✔ Security (Encryption at rest & in transit)
- ✔ Monitoring (CloudWatch integration)

---

## 10. Types of Deployments

### Single-AZ Deployment
- Simple and cheap
- Only 1 DB in one Availability Zone
- Risk: Downtime if server crashes

### Multi-AZ Deployment
- High Availability
- Automatic Failover (backup DB in another AZ)
- Used for production

### Read Replicas
- Used for heavy read operations (reporting, analytics)
- Improves performance

---

## 11. Security in RDS
- Use **IAM** for user permission
- Use **VPC** and **Subnets** for network isolation
- Use **Encryption** for data at rest and in transit (SSL)
- Use **Security Groups** like firewalls

---

## 12. Monitoring Tools
- **Amazon CloudWatch** – Check CPU, memory, connections, etc.
- **Enhanced Monitoring** – OS-level metrics (requires agent)
- **Performance Insights** – Analyze slow queries and bottlenecks

---

## 13. Common Tasks
- Create/Delete DB
- Modify instance size or storage
- Take backups
- Restore from backup or snapshot
- Monitor logs and performance
- Set up alerts using CloudWatch

---