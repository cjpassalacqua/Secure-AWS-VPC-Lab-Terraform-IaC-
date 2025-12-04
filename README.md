# Secure AWS VPC Lab (Terraform Infrastructure as Code)

A fully automated, production-style **secure AWS network** deployed using **Terraform**.  
This project demonstrates end-to-end cloud security architecture, including VPC design, subnet isolation, routing, logging, and foundational threat detection.

The goal: build a cloud environment the way a **Cloud Security Engineer** or **DevSecOps Engineer** would — using code, least-privilege access, encrypted logging, and layered network protections.

---

## 🚀 Project Overview

This Terraform configuration deploys a secure networking baseline inside AWS:

- Custom VPC with controlled IPv4 address space  
- Segmented **public and private subnets** across multiple Availability Zones  
- **Internet Gateway (IGW)** for inbound/outbound internet  
- **NAT Gateway** for secure outbound access from private subnets  
- **Route tables** for traffic segmentation  
- **Security groups** applying least-privilege rules  
- **S3 bucket for VPC Flow Logs** (network traffic logging)  
- **GuardDuty threat detection** enabled in the region  
- Fully reproducible **Infrastructure as Code** (IaC)

Perfect as a foundation for EC2, EKS, RDS, or additional security automation modules.

---

## 🛡️ Security Features

### ✔ Network Segmentation  
- Public subnets for internet-facing resources  
- Private subnets for internal workloads  
- NAT Gateway ensures private resources never receive inbound internet traffic  

### ✔ Logging & Visibility  
- VPC Flow Logs delivered to a dedicated S3 bucket  
- S3 server-side encryption enabled  
- Least-privilege IAM for logging services (if extended to CWL)

### ✔ Threat Detection
- AWS GuardDuty enabled for:
  - Anomalous behavior detection  
  - Reconnaissance alerts  
  - S3 threat detection  
  - IAM/credential abuse indicators  

### ✔ Infrastructure as Code  
- Full environment deployed with a single command  
- Reproducible + version controlled in Git  
- Eliminates manual console misconfigurations

---

## 🧱 Architecture Diagram

```

┌──────────────────────────────────────────────┐
│                    VPC                       │
│                 (10.0.0.0/16)                │
│                                              │
│  ┌──────────────┐     ┌──────────────┐       │
│  │ Public Subnet│     │ Public Subnet│       │
│  │ (AZ a)       │     │ (AZ b)       │       │
│  └──────┬───────┘     └──────┬───────┘       │
│         │ IGW                │               │
│         └──────────┬─────────┘               │
│                    │                         │
│        ┌───────────▼───────────┐             │
│        │     Route Table       │             │
│        │ 0.0.0.0/0 → IGW       │             │
│        └────────────────────────┘            │
│                                              │
│  ┌──────────────┐     ┌──────────────┐       │
│  │Private Subnet│     │Private Subnet│       │
│  │ (AZ a)       │     │ (AZ b)       │       │
│  └──────┬───────┘     └──────┬───────┘       │
│         │ NAT                │               │
│         └──────────┬─────────┘               │
│                    │                         │
│        ┌───────────▼───────────┐             │
│        │     Route Table       │             │
│        │ 0.0.0.0/0 → NAT       │             │
│        └────────────────────────┘            │
│                                              │
└──────────────────────────────────────────────┘

```

---

## 🗂️ Repository Structure

```

tf-secure-vpc/
│
├── main.tf          # All VPC, subnets, routing, flow logs, SGs
├── variables.tf     # Reusable variables for customization
├── outputs.tf       # Export useful information after deployment
└── README.md        # Documentation (this file)

````

---

## ⚙️ Deployment Instructions

### 1. Install Requirements
- Terraform v1.5+
- AWS CLI v2
- Configured AWS credentials (`aws configure`)

### 2. Clone the Repository

```bash
git clone https://github.com/<your-username>/tf-secure-vpc.git
cd tf-secure-vpc
````

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Review the execution plan

```bash
terraform plan
```

### 5. Apply configuration

```bash
terraform apply
```

Type **yes** to confirm.

### 6. Destroy (Optional)

```bash
terraform destroy
```

---

## 🔍 Outputs

Terraform will return helpful metadata:

* **VPC ID**
* **Public Subnet IDs**
* **Private Subnet IDs**
* **Flow Logs S3 Bucket Name**

---

## 🧠 What I Learned

* Designing secure VPC network topologies
* Implementing infrastructure using Terraform
* Managing public vs private subnets
* Using NAT gateways for secure outbound traffic
* Enabling VPC Flow Logs for network visibility
* Enforcing least-privilege architecture
* Understanding GuardDuty threat detection
* Applying infrastructure-as-code best practices
* Debugging and resolving Terraform errors
* Following AWS security best practices for baseline environments

---

## 📌 Next Steps & Enhancements

Planned upgrades:

* Add CloudTrail + KMS encryption
* Add KMS-encrypted S3 buckets
* Add ALB + private EC2 web tier
* Turn this VPC into a reusable module
* Add IAM role hardening and SCP policies
* Simulate GuardDuty findings for training

---

## 🧑‍💻 Author

Christian Passalacqua
Cloud Security | AWS | Terraform | DevSecOps

---

## ⭐ Like this project?

Feel free to fork it, improve it, or use it in your own learning journey!

```
