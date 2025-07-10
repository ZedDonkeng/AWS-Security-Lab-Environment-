# AWS Security Architecture Labs 🔒☁️

*Implementing production-grade AWS security best practices through automated, hands-on labs.  
Aligned with the AWS Well-Architected Framework and CIS Benchmarks.*

[![AWS Certified](https://img.shields.io/badge/AWS-Certified%20Solutions%20Architect-orange)](https://www.credly.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.9%2B-brightgreen)](https://www.python.org/)

## 🛡️ Lab Overview

This repository contains practical implementations of AWS security controls for:
- **Secure multi-tier architectures**
- **Automated compliance monitoring**
- **Real-time threat detection**
- **Data protection at scale**

Designed for cloud security professionals who want to learn through infrastructure-as-code.

## 🧪 Labs Catalog

### 1. [Secure Multi-Tier VPC](https://github.com/ZedDonkeng/AWS-Security-Lab-Environment-/blob/main/Secure%20Multi-Tier%20VPC.md)
- [AWS NAT GETWAY](https://github.com/ZedDonkeng/AWS-Security-Lab-Environment-/blob/main/NAT-Gateway%20on%20aws.md)
  
🔧 *Tech: CloudFormation, NACLs, WAF*  
✅ Implements zero-trust networking with:  
- Public/private/isolated subnets  
- Application Load Balancer with WAF protection  
- VPC Flow Logs integration  

### 2. [Automated Security Baseline](https://github.com/ZedDonkeng/AWS-Security-Lab-Environment-/blob/main/Automated%20Security%20Baseline.md)
🔧 *Tech: AWS Organizations, SCPs, AWS Config*  
✅ Enforces:  
- Service Control Policies (SCPs) to block risky APIs  
- IAM permission boundaries  
- MFA enforcement for all IAM users  

### 3. [Threat Detection Pipeline](https://github.com/ZedDonkeng/AWS-Security-Lab-Environment-/blob/main/Threat%20Detection%20Pipeline.md)
🔧 *Tech: GuardDuty, Lambda, Security Hub*  
✅ Implements:  
- Automated containment of compromised IAM users  
- CloudWatch anomaly detection  
- Security Hub custom insights  

### 4. [Data Protection Workshop](data-protection/)
🔧 *Tech: KMS, Macie, S3 Bucket Policies*  
✅ Features:  
- S3 object encryption with KMS CMKs  
- Automated PII discovery with Macie  
- Cross-account audit trails  

## 🚀 Quick Start

1. **Prerequisites**:
   - AWS Account with Admin permissions
   - AWS CLI configured
   - Python 3.9+

2. **Deploy a Lab**:
```bash
cd multi-tier-vpc
aws cloudformation deploy --template-file security-vpc.yml --stack-name SecureVPC
