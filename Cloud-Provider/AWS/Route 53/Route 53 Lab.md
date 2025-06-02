
# AWS Route 53 Hands-on Lab Guide (Easy Step-by-Step)

## Lab 1: Registering a Domain (Simulated)
### Simulate Domain Registration:
- Navigate to **Route 53 Console**: AWS Management Console → Services → Route 53
- Go to **"Domain registration" → "Register domain"**
- Type a test domain (e.g., `myawslabtest123.com`)
- ⚠️ Note: Actual registration requires payment.

### Alternative:
- Create a hosted zone for a domain you own or use `example.com`.

---

## Lab 2: Creating Hosted Zones
### Create Public Hosted Zone:
- Go to **Route 53 Console → Hosted zones → Create hosted zone**
- Domain name: `example.com`
- Type: Public
- Click **Create**

### Record Sets Created Automatically:
- **NS (Name Server)** records
- **SOA (Start of Authority)** record

---

## Lab 3: Creating DNS Records
### Add A Record (IPv4):
- Click **Create record**
- Record name: `www`
- Type: A - IPv4 address
- Value: `192.0.2.1`
- TTL: 300
- Click **Create records**

### Add CNAME Record:
- Create record
- Name: `mail`
- Type: CNAME
- Value: `gmail.com`
- Click **Create**

### Add Alias Record:
- Create record
- Name: `cdn`
- Type: A - IPv4
- Toggle **Alias**: ON
- Select AWS resource (e.g., CloudFront)
- Click **Create**

---

## Lab 4: Routing Policies Demo
### Weighted Routing:
- Create two A records for `app.example.com`
  - Record 1: Weight 70 → IP `192.0.2.10`
  - Record 2: Weight 30 → IP `192.0.2.20`
- Simulates 70/30 traffic distribution

### Failover Routing:
- Primary: `primary.example.com` → Type: Failover PRIMARY
  - Associate Health Check
- Secondary: `secondary.example.com` → Type: Failover SECONDARY

### Latency-Based Routing:
- Record 1 (US): `192.0.2.100`
- Record 2 (EU): `192.0.2.200`
- Route 53 routes to nearest region

---

## Lab 5: Health Checks
### Create Health Check:
- Go to **Route 53 → Health checks → Create**
- Endpoint: Website URL or IP
- Protocol: HTTP/HTTPS
- Path: `/health`
- Interval: 30s, Threshold: 3 failures

### Associate with Record:
- Edit DNS record
- Attach the health check
- Save

---

## Lab 6: Private Hosted Zone (VPC DNS)
### Create Private Hosted Zone:
- Hosted zone name: `internal.example.com`
- Type: Private
- Associate with VPC(s)
- Click **Create**

### Add Private Records:
- A record: `db.internal.example.com` → Value: `10.0.1.50`

---

## Lab 7: Route 53 Resolver (Hybrid DNS)
### Create Inbound Endpoint:
- Go to **Route 53 → Resolver → Inbound endpoints**
- Select VPC
- Choose 2 subnets (in different AZs)
- Use security group allowing TCP/UDP 53

### Create Outbound Endpoint:
- Go to **Outbound endpoints**
- Similar steps as inbound
- Add rule to forward to on-prem DNS

---

## Cleanup
- Delete all test hosted zones
- Delete health checks
- Delete resolver endpoints
- ⚠️ Registered domains can't be deleted — they expire.

---

## Pro Tips
- **Free Tier**:
  - 25 hosted zones free
  - 1M queries/month free

- **Testing Tools**:
  - `dig example.com`
  - `nslookup example.com`

- **Visual Aids**:
  - Enable Route 53 Traffic Flow visual editor
  - Use AWS Diagrams

- **Common Mistakes**:
  - Forgetting to update parent NS records
  - High TTLs during testing
  - Not enabling DNSSEC when required