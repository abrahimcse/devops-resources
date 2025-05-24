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


## 🧹 How to Delete Auto Scaling Setup in AWS

Follow these steps to safely delete your Auto Scaling Group, associated Launch Template, and CloudWatch alarms.

### ✅ Step 1: Update Auto Scaling Group to Prevent New Instances

```bash
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name <your-asg-name> \
  --min-size 0 \
  --max-size 0 \
  --desired-capacity 0
```

### ✅ Step 2: Delete the Auto Scaling Group (ASG)

Via AWS Console:

* Go to **EC2 Dashboard > Auto Scaling Groups**
* Select your Auto Scaling Group
* Click **Delete** at the top

Or use CLI:

```bash
aws autoscaling delete-auto-scaling-group \
  --auto-scaling-group-name <your-asg-name> \
  --force-delete
```

### ✅ Step 3: Delete the Launch Template (Optional)

Via Console:

* Go to **EC2 > Launch Templates**
* Select the template used in the ASG
* Click **Actions > Delete**

Via CLI:

```bash
aws ec2 delete-launch-template \
  --launch-template-name <your-template-name>
```

### ✅ Step 4: Delete CloudWatch Alarms (Optional)

Via Console:

* Go to **CloudWatch > Alarms**
* Select the alarms tied to scaling
* Click **Actions > Delete**

Via CLI:

```bash
aws cloudwatch delete-alarms \
  --alarm-names <alarm-name-1> <alarm-name-2>
```

### ⚠️ Important Notes

* Make sure no production workloads are using the ASG before deleting.
* If any instance was manually added to the ASG, it will also be terminated.
* You can also delete Load Balancers if they were created for Auto Scaling.

## ✅ Final Cleanup Checklist

| Resource           | Action               |
| ------------------ | -------------------- |
| Auto Scaling Group | ✅ Deleted            |
| Launch Template    | ✅ Deleted (Optional) |
| CloudWatch Alarms  | ✅ Deleted (Optional) |
| Instances          | ✅ Terminated via ASG |

---

## ✨ Final Thought

"Use Auto Scaling to build resilient, responsive, and cost-efficient cloud applications — a must-have skill for DevOps Engineers."

---

