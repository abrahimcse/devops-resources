# 🛠️ AWS Auto Scaling - Step by Step Guide

## 📘 What is AWS Auto Scaling?

AWS Auto Scaling helps you automatically adjust the number of Amazon EC2 instances in your application environment based on demand. This ensures high availability and reduces costs.


## 🪜 Step-by-Step Setup of EC2 Auto Scaling

### ✅ Step 1: Create a Launch Template

Go to **EC2 Dashboard > Launch Templates > Create Launch Template**

* Add basic details (name, version)
* Select AMI
* Choose instance type
* Add key pair, security group, etc.

### ✅ Step 2: Create Auto Scaling Group

Go to **EC2 Dashboard > Auto Scaling Groups > Create Auto Scaling Group**

* Use the launch template created in Step 1
* Configure VPC, subnet
* Set minimum, maximum, and desired capacity
* Choose load balancing if needed (ELB or ALB)
* Set health check type (EC2 or ELB)
* Configure scaling policies (next step)

### ✅ Step 3: Configure Scaling Policies

From within the ASG settings:

* Go to **Automatic Scaling > Add Policy**
* Choose one:

  * Target tracking
  * Step scaling
  * Scheduled scaling

🎯 Example (Target tracking):

* Target value: Average CPU 50%
* Scale-out/in cooldown: 300s

### ✅ Step 4: Set CloudWatch Alarm (Optional but Recommended)

Go to **CloudWatch > Alarms > Create Alarm**

* Select metric: EC2 > Per-Instance Metrics > CPUUtilization
* Set threshold: e.g., CPU > 70% for 2 evaluations
* Action: Attach it to Auto Scaling Policy

---

## 📈 Example Use Cases

* Flash sale traffic spike → Auto Scaling scales **out**
* Low traffic at night → Auto Scaling scales **in**

---

## 📂 CLI Examples

### 🔧 Create Auto Scaling Group

```bash
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template \
  --min-size 2 \
  --max-size 10 \
  --desired-capacity 3 \
  --vpc-zone-identifier subnet-abc12345
```

### 🔍 Create CloudWatch Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu-alarm \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 60 \
  --threshold 70 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:autoscaling:...:policy/scale-out \
  --dimensions Name=AutoScalingGroupName,Value=my-asg
```

---

## 🔗 Integration Tips

* ✅ With **ELB**: Load balances across all instances
* ✅ With **CloudWatch**: Drive metrics-based scaling
* ✅ With **Terraform**: Manage via Infrastructure as Code (IaC)
