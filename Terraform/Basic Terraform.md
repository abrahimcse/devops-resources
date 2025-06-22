# **Terraform: Infrastructure as Code (IaC)**  

Terraform is an open-source Infrastructure as Code (IaC) tool developed by **Mitchell Hashimoto** in July 2014. It enables efficient management of infrastructure across multiple cloud providers and hybrid cloud environments.  

---
## Project Structure

```
terraform-project/
├── modules/               # Reusable Terraform modules
│   ├── networking/        # VPC, subnets, etc.
│   ├── compute/           # EC2, VM instances
│   └── database/          # RDS, Cloud SQL
├── environments/          # Environment-specific configs
│   ├── dev/               # Development environment
│   ├── staging/           # Staging environment
│   └── prod/              # Production environment
├── main.tf                # Primary configuration
├── variables.tf           # Input variables
├── outputs.tf             # Output variables
└── README.md              # This file
```

## **Key Features**
- **Multi-cloud and Hybrid Cloud** support.  
- Written in **Go** programming language.  
- Uses **HashiCorp Configuration Language (HCL)**, which is easy to read and write.  
- Declarative approach to infrastructure provisioning.  

---

## **What is Infrastructure as Code (IaC)?**
**IaC** automates the management and provisioning of infrastructure using code instead of manual processes.  

### **Types of IaC**:
1. **Imperative**:  
   - Requires arranging tasks in the correct sequence.  
   - Example: AWS CloudFormation.  

2. **Declarative**:  
   - Focuses on defining the desired state without worrying about the order of tasks.  
   - Example: Terraform.  

---

## **IaC Tools**
| **Cloud Providers**      | **IaC Tools**                 |  
|--------------------------|-------------------------------|  
| **AWS**                  | CloudFormation (2011)         |  
| **Azure**                | ARM Templates                 |  
| **Google Cloud Platform**| Deployment Manager            |  
| **Open Source**          | Terraform, Ansible            |  

---

## **Terraform vs Configuration Management Tools (CMT)**
| **Category**        | **Purpose**                                                    | **Examples**              |  
|---------------------|----------------------------------------------------------------|---------------------------|  
| **IaC Tools**       | Provision servers and infrastructure.                          | Terraform, CloudFormation |  
| **CMT Tools**       | Install and manage software on existing servers.               | Ansible, Chef, Puppet     |  

**Tip**: IaC and CMT can be combined for robust infrastructure and software management.  

---

## **Terraform Commands**
- **`terraform init`**: Initializes the Terraform environment.  
- **`terraform plan`**: Creates an execution plan to preview infrastructure changes.  
- **`terraform validate`**: Validates the configuration files.  
- **`terraform apply`**: Applies the configuration to provision infrastructure.  
- **`terraform destroy`**: Destroys all provisioned resources.  

---

## **Terraform Architecture**
![Terraform Architecture](https://github.com/abrahimcse/devops-resources/blob/main/Terraform/Images/terraform-architecture-diagram.png)

---

## **Terraform Syntax**  
Terraform configurations are written in **HCL (HashiCorp Configuration Language)**.  

### **HCL Basics**:  
1. **Blocks**: The primary structural element.  
2. **Arguments**: Key-value pairs (e.g., `key = value`).  
3. **Expressions**: Define resources and configurations.  

HCL is designed to be human-readable, concise, and efficient for defining infrastructure.  

---

## **Fun Fact**  
The name "Terraform" is inspired by the concept of making a planet like Earth (Terraforming), aligning with its goal of making infrastructure management seamless.  
