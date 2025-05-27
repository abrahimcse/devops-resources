
# 📘 AWS DynamoDB Full Guide (Easy Explanation)

## 🧾 What is DynamoDB?
Amazon DynamoDB is a **NoSQL managed database** service by AWS.
- **Fast and scalable** (supports millions of requests per second).
- No need to manage servers.
- Stores data as **tables with items and attributes** (like rows and columns, but schema-less).

---

## 🧱 Basic Concepts

### 🔹 Table
- Like a database table.
- Contains **Items**.

### 🔹 Item
- Like a row in RDBMS.
- Made of **attributes**.

### 🔹 Attribute
- A data field (like a column), e.g., Name, Age, ID.

### 🔹 Primary Key
- Used to uniquely identify items.
- Two types:
  - **Partition Key** only (e.g., `UserID`).
  - **Partition + Sort Key** (e.g., `UserID + Timestamp`).

---

## ⚙️ Read/Write Capacity Units (RCU/WCU)

### 🔹 Read Capacity Unit (RCU)
- **1 RCU** = 1 strongly consistent read per second for item up to 4 KB.
- **Eventually consistent** reads = ½ RCU.

### 🔹 Write Capacity Unit (WCU)
- **1 WCU** = 1 write per second for item up to 1 KB.

---

## 💰 Pricing Overview

- **On-Demand Mode** (pay-per-request):
  - Good for unpredictable traffic.
  - Charged per RCU/WCU used.

- **Provisioned Mode**:
  - You set RCU/WCU values.
  - Good for predictable traffic.
  - Can enable auto-scaling.

- **Other costs**:
  - Data storage: per GB/month.
  - Backup and restore.
  - Streams (real-time change tracking).
  - Global tables (multi-region replication).

---


## 🏗️ Create Table (AWS Console)
1. In the **Search bar**, type `DynamoDB`.
2. Click on **DynamoDB** service.
3. Go to DynamoDB service.
4. Click **"Create Table"**.
5. Set:
   - Table name: `Users`
   - Partition key: `UserID` (String)
   - Optional: Sort key (e.g., `Timestamp`)
6. Select billing mode: On-demand / Provisioned.
7. Click **Create**.

Table will be created in a few seconds.

---

## ➕ Add Items (Records)

- Go to table → **Explore table items**.
- Click **"Create item"**.
- Add attributes like:
```json
{
  "UserID": "user01",
  "Name": "Abdur Rahim",
  "Age": 28,
  "City": "Dhaka"
}
```

---

## 🔍 Query vs Scan

### 🔎 Query
- **Fast and efficient**.
- Needs partition key (and optional sort key).
```json
KeyConditionExpression: "UserID = :uid"
ExpressionAttributeValues: {
  ":uid": "user01"
}
```

### 🔍 Scan
- Reads **all items** in the table.
- Slower and more expensive.
```json
FilterExpression: "City = :city"
ExpressionAttributeValues: {
  ":city": "Dhaka"
}
```

---

## 🛠️ Common Operations

### 🔹 Add Item (PutItem)
```json
{
  "UserID": "user02",
  "Name": "Hridoy",
  "Email": "hridoy@example.com"
}
```

### 🔹 Update Item
- Use `UpdateItem` API.
- Can update specific attributes.

### 🔹 Delete Item
- Use `DeleteItem` with key.

---

## 🔐 Security & Access
- Use IAM roles & policies.
- Fine-grained access control possible.

---

## 📊 Monitoring
- CloudWatch metrics available (RCU/WCU usage, throttling, etc.).

---

## 🧹 Clean Up
- Delete unused tables to avoid charges.

---

## 🧠 Summary

| Feature             | Description                          |
|---------------------|--------------------------------------|
| Type                | NoSQL (Key-Value & Document)         |
| Scalability         | Automatic                            |
| Pricing             | On-Demand or Provisioned             |
| Key Components      | Table, Item, Attribute               |
| Query vs Scan       | Query is efficient, Scan is full scan|
| Security            | IAM + Encryption                     |