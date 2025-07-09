
# AWS WAF (Web Application Firewall) Setup Guide

## Overview
This tutorial demonstrates how to configure AWS WAF with an Application Load Balancer (ALB) to protect an EC2 instance, including IP-based traffic blocking.

## Architecture
- **VPC**: `demo-vpc-2023` (CIDR: `10.1.0.0/16`)
- **Public Subnets**:
  - `demo-subnet-alpha` (AZ: `us-east-1a`, CIDR: `10.1.1.0/24`)
  - `demo-subnet-beta` (AZ: `us-east-1b`, CIDR: `10.1.2.0/24`)
- **EC2 Instance**: `waf-protected-server`
- **IP to Block**: `203.0.113.42` (example test IP)

## Step-by-Step Configuration

### 1. Create a VPC
```bash
1. Go to VPC Dashboard > Create VPC
2. Configure:
   - Name: demo-vpc-2023
   - IPv4 CIDR: 10.1.0.0/16
3. Click Create
```

### 2. Set Up Internet Gateway
```bash
1. Navigate to Internet Gateways > Create Internet Gateway
2. Name: demo-igw
3. Attach to demo-vpc-2023
```

### 3. Create Subnets
```bash
1. Under Subnets, create:
   - Subnet 1:
     - Name: demo-subnet-alpha
     - VPC: demo-vpc-2023
     - AZ: us-east-1a
     - CIDR: 10.1.1.0/24
   - Subnet 2:
     - Name: demo-subnet-beta
     - AZ: us-east-1b
     - CIDR: 10.1.2.0/24
```

### 4. Configure Route Tables
```bash
1. Create Public Route Table:
   - Name: demo-public-rt
   - Add route: 0.0.0.0/0 → demo-igw
2. Associate both subnets
```

### 5. Launch an EC2 Instance
```bash
1. AMI: Amazon Linux 2023
2. Instance Type: t3.micro
3. Key Pair: waf-demo-key
4. Network Settings:
   - VPC: demo-vpc-2023
   - Subnet: demo-subnet-alpha
   - Auto-assign Public IP: Enabled
5. Security Group: Allow HTTP (80) and SSH (22)
6. User Data:
   #!/bin/bash
   yum update -y
   yum install httpd -y
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Protected by AWS WAF</h1>" > /var/www/html/index.html
```

### 6. Set Up ALB & Target Group
```bash
1. Target Group:
   - Name: demo-tg-ec2
   - Protocol: HTTP (80)
   - Attach EC2 instance
2. Application Load Balancer:
   - Name: demo-alb-waf
   - Scheme: Internet-facing
   - Listeners: HTTP (80) → demo-tg-ec2
```

### 7. Configure AWS WAF
```bash
1. Create IP Set:
   - Name: blocked-ips
   - IP: 203.0.113.42/32
2. Web ACL Rules:
   - Name: block-malicious-ips
   - Rule Type: IP Match
   - Action: Block
   - IP Set: blocked-ips
3. Associate ALB: Attach demo-alb-waf
```

## Testing
1. **From Allowed IP**: Access ALB DNS → Success (Apache page loads)
2. **From Blocked IP (203.0.113.42)**: HTTP 403 Forbidden

## Next Steps
- Try geo-blocking or SQL injection rules
- Explore VPC Peering in our next tutorial
