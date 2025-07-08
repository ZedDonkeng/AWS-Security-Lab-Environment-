---
title: Secure VPC Architecture
---

# 🔐 Secure AWS VPC Architecture

Welcome to my project showcasing a **secure Amazon VPC (Virtual Private Cloud)** design, aligned with AWS best practices for cloud security and network segmentation.

<p align="center">
  <img src="https://github.com/user-attachments/assets/d0953b7c-1ff4-4445-bc63-f7f014832cf7" alt="Secure VPC Architecture" width="720" height="400">
</p>

---

## 🧱 Architecture Overview

This VPC includes:

- **Public Subnet**  
  - NAT Gateway  
  - Bastion Host (optional)

- **Private Subnet**  
  - EC2 Instances  
  - RDS Databases  
  - Internal-only services

- **Route Tables**  
  - Route Internet-bound traffic through NAT
  - Isolate private resources from direct internet access

- **Security Groups & NACLs**  
  - Layered control at instance and subnet level

- **VPC Flow Logs**  
  - Monitors accepted/rejected traffic for auditing

- **IAM Roles & Policies**  
  - Limits access based on principle of least privilege

---

## 🔐 Security Highlights

✅ Isolated subnets for internal services  
✅ Minimal public exposure  
✅ Logging and traffic analysis enabled  
✅ Tight security group rules  
✅ Infrastructure-as-Code with Terraform

---

## 🧪 Hands-On Labs Included

```bash
📁 terraform/
├── main.tf         # VPC, subnets, route tables
├── security.tf     # SGs, NACLs
├── variables.tf
