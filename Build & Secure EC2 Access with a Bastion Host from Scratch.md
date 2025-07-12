# AWS Bastion Host Lab Guide
## Build & Secure EC2 Access with a Bastion Host from Scratch

This comprehensive lab guide will walk you through creating a secure AWS environment where a bastion host provides SSH access to a private EC2 instance with internet connectivity.

---

## Prerequisites

Before starting this lab, ensure you have:

- **AWS Account** with console access
- **SSH Key Pair** for EC2 access (or ability to create one)
- **Basic Linux Command Line** familiarity
- **Your Public IP Address** (for security group configuration - we'll use 203.0.113.0/32 as example)
- **Region**: us-east-1 (N. Virginia)

---

## Architecture Overview
You will build a secure network architecture consisting of:

### Network Components
- **1 VPC** with custom IP range (10.0.0.0/16)
- **2 Subnets**:
  - Public Subnet (10.0.1.0/24) - for bastion host
  - Private Subnet (10.0.2.0/24) - for internal EC2 instance
- **1 Internet Gateway** - for public internet access
- **1 NAT Gateway** - for private subnet internet access
- **Route Tables** - for traffic routing

### Compute Resources
- **Bastion Host** (public subnet, public IP)
- **Private Instance** (private subnet, no public IP)

### Security Components
- **Bastion Security Group** - allows SSH from your IP only
- **Private Security Group** - allows SSH only from Bastion Security Group

---

## Step 1: Create VPC and Network Foundation

### 1.1 Create the VPC

1. Navigate to **VPC Console** → **Your VPCs** → **Create VPC**
2. Configure the VPC:
   - **Name**: `SecureLabVPC`
   - **IPv4 CIDR block**: `10.0.0.0/16`
   - **Tenancy**: Default
3. Click **Create VPC**

### 1.2 Create Public Subnet

1. Navigate to **Subnets** → **Create subnet**
2. Configure the public subnet:
   - **Name**: `WebSubnet`
   - **VPC**: Select `SecureLabVPC`
   - **Availability Zone**: `us-east-1a`
   - **IPv4 CIDR block**: `10.0.1.0/24`
3. Click **Create subnet**

### 1.3 Create Private Subnet

1. Navigate to **Subnets** → **Create subnet**
2. Configure the private subnet:
   - **Name**: `AppSubnet`
   - **VPC**: Select `SecureLabVPC`
   - **Availability Zone**: `us-east-1a`
   - **IPv4 CIDR block**: `10.0.2.0/24`
3. Click **Create subnet**

---

## Step 2: Configure Internet Access for Public Subnet

### 2.1 Create Internet Gateway

1. Navigate to **Internet Gateways** → **Create internet gateway**
2. Configure the gateway:
   - **Name**: `MainIGW`
3. Click **Create internet gateway**
4. **Attach to VPC**:
   - Select the created gateway
   - Click **Actions** → **Attach to VPC**
   - Select `SecureLabVPC`

### 2.2 Create Public Route Table

1. Navigate to **Route Tables** → **Create route table**
2. Configure the route table:
   - **Name**: `WebRT`
   - **VPC**: Select `SecureLabVPC`
3. Click **Create route table**

### 2.3 Configure Public Routing

1. Select the `WebRT` route table
2. Click **Routes** tab → **Edit routes**
3. Add internet route:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select `Internet Gateway` → `MainIGW`
4. Click **Save changes**

### 2.4 Associate Public Subnet

1. In the `WebRT` route table, click **Subnet associations** tab
2. Click **Edit subnet associations**
3. Select `WebSubnet`
4. Click **Save associations**

---

## Step 3: Create Security Groups

### 3.1 Create Bastion Security Group

1. Navigate to **EC2** → **Security Groups** → **Create security group**
2. Configure the security group:
   - **Name**: `JumpboxSG`
   - **Description**: `Security group for bastion host`
   - **VPC**: Select `SecureLabVPC`
3. Configure **Inbound Rules**:
   - **Type**: SSH
   - **Protocol**: TCP
   - **Port**: 22
   - **Source**: 203.0.113.0/32 (example IP - replace with your actual IP)
4. **Outbound Rules**: Leave default (allow all)
5. Click **Create security group**

### 3.2 Create Private Security Group

1. Navigate to **EC2** → **Security Groups** → **Create security group**
2. Configure the security group:
   - **Name**: `InternalSG`
   - **Description**: `Security group for private instances`
   - **VPC**: Select `SecureLabVPC`
3. Configure **Inbound Rules**:
   - **Type**: SSH
   - **Protocol**: TCP
   - **Port**: 22
   - **Source**: Custom → Select `JumpboxSG` security group
4. **Outbound Rules**: Leave default (allow all)
5. Click **Create security group**

---

## Step 4: Launch EC2 Instances

### 4.1 Create SSH Key Pair (if needed)

1. Navigate to **EC2** → **Key Pairs** → **Create key pair**
2. Configure the key pair:
   - **Name**: `lab-keypair`
   - **Key pair type**: RSA
   - **Private key file format**: .pem
3. Click **Create key pair**
4. **Save the .pem file securely** on your local machine

### 4.2 Launch Bastion Host

1. Navigate to **EC2** → **Instances** → **Launch instance**
2. Configure the instance:
   - **Name**: `JumpboxHost`
   - **AMI**: Amazon Linux 2 AMI (HVM) - SSD Volume Type
   - **Instance type**: t2.micro
   - **Key pair**: Select `lab-keypair`
3. Configure **Network settings**:
   - **VPC**: Select `SecureLabVPC`
   - **Subnet**: Select `WebSubnet`
   - **Auto-assign public IP**: Enable
   - **Security group**: Select existing → `JumpboxSG`
4. Click **Launch instance**

### 4.3 Launch Private Instance

1. Navigate to **EC2** → **Instances** → **Launch instance**
2. Configure the instance:
   - **Name**: `AppServer`
   - **AMI**: Amazon Linux 2 AMI (HVM) - SSD Volume Type
   - **Instance type**: t2.micro
   - **Key pair**: Select `lab-keypair`
3. Configure **Network settings**:
   - **VPC**: Select `SecureLabVPC`
   - **Subnet**: Select `AppSubnet`
   - **Auto-assign public IP**: Disable
   - **Security group**: Select existing → `InternalSG`
4. Click **Launch instance**

---

## Step 5: Enable Internet Access for Private Instance

### 5.1 Allocate Elastic IP

1. Navigate to **VPC** → **Elastic IPs** → **Allocate Elastic IP address**
2. Click **Allocate**
3. Note the allocated Elastic IP address

### 5.2 Create NAT Gateway

1. Navigate to **VPC** → **NAT Gateways** → **Create NAT gateway**
2. Configure the NAT gateway:
   - **Name**: `AppNATGateway`
   - **Subnet**: Select `WebSubnet`
   - **Connectivity type**: Public
   - **Elastic IP allocation ID**: Select the Elastic IP from Step 5.1
3. Click **Create NAT gateway**
4. Wait for status to become "Available" (this may take a few minutes)

### 5.3 Create Private Route Table

1. Navigate to **Route Tables** → **Create route table**
2. Configure the route table:
   - **Name**: `AppRT`
   - **VPC**: Select `SecureLabVPC`
3. Click **Create route table**

### 5.4 Configure Private Routing

1. Select the `AppRT` route table
2. Click **Routes** tab → **Edit routes**
3. Add internet route:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select `NAT Gateway` → `AppNATGateway`
4. Click **Save changes**

### 5.5 Associate Private Subnet

1. In the `AppRT` route table, click **Subnet associations** tab
2. Click **Edit subnet associations**
3. Select `AppSubnet`
4. Click **Save associations**

---

## Step 6: Test SSH Access and Connectivity

### 6.1 Prepare SSH Key

On your local machine, set the correct permissions for your SSH key:

```bash
chmod 400 lab-keypair.pem
```

### 6.2 Connect to Bastion Host

1. Get the public IP of your Jumpbox Host from the EC2 console (example: 54.230.142.85)
2. Connect from your local terminal:

```bash
ssh -i lab-keypair.pem ec2-user@54.230.142.85
```

**Expected Result**: You should successfully connect to the bastion host.

### 6.3 Transfer SSH Key to Bastion (Optional Method)

If you want to SSH from bastion to private instance, you can:

1. Copy the private key to the jumpbox host:
```bash
scp -i lab-keypair.pem lab-keypair.pem ec2-user@54.230.142.85:~/.ssh/
```

2. Set permissions on the jumpbox host:
```bash
chmod 400 ~/.ssh/lab-keypair.pem
```

### 6.4 Connect to Private Instance via Bastion

1. From inside the Jumpbox Host, get the private IP of `AppServer` from AWS console (example: 10.0.2.45)
2. Connect to the private instance:

```bash
ssh -i ~/.ssh/lab-keypair.pem ec2-user@10.0.2.45
```

**Expected Result**: You should successfully connect to the private instance.

### 6.5 Test Internet Access from Private Instance

From the private instance, test internet connectivity:

```bash
# Test DNS resolution and connectivity
ping -c 4 google.com

# Test package updates
sudo yum update -y

# Test downloading from internet
curl -I https://www.google.com
```

**Expected Result**: All commands should work, confirming internet access.

### 6.6 Verify Security (Optional)

Try to connect directly to the private instance from your local machine:

```bash
ssh -i lab-keypair.pem ec2-user@10.0.2.45
```

**Expected Result**: This should fail, as the private instance has no public IP and only accepts SSH from the bastion host.

---

## Step 7: Cleanup Resources (Optional)

When you're finished with the lab, clean up resources to avoid charges:

### 7.1 Terminate EC2 Instances
1. Navigate to **EC2** → **Instances**
2. Select both instances
3. Click **Instance state** → **Terminate instance**

### 7.2 Delete NAT Gateway
1. Navigate to **VPC** → **NAT Gateways**
2. Select `AppNATGateway`
3. Click **Actions** → **Delete NAT gateway**

### 7.3 Release Elastic IP
1. Navigate to **VPC** → **Elastic IPs**
2. Select the allocated IP
3. Click **Actions** → **Release Elastic IP addresses**

### 7.4 Delete Network Components
Delete in this order:
1. **Route Tables** (custom ones only)
2. **Security Groups** (custom ones only)
3. **Subnets**
4. **Internet Gateway** (detach first, then delete)
5. **VPC**

---

## Troubleshooting Common Issues

### Issue 1: Cannot Connect to Bastion Host
- **Check**: Security group allows SSH from your current IP
- **Check**: Instance is in running state
- **Check**: Public IP is assigned
- **Check**: Route table has internet gateway route

### Issue 2: Cannot Connect to Private Instance from Bastion
- **Check**: Private security group allows SSH from bastion security group
- **Check**: Private instance is in running state
- **Check**: Using correct private IP address

### Issue 3: Private Instance Has No Internet Access
- **Check**: NAT Gateway is in "Available" state
- **Check**: Private route table has NAT Gateway route (0.0.0.0/0)
- **Check**: Private subnet is associated with private route table
- **Check**: Private security group allows outbound traffic

### Issue 4: SSH Key Permission Denied
- **Check**: Key file permissions are set to 400
- **Check**: Using correct key file
- **Check**: Using correct username (ec2-user for Amazon Linux)

---

## Lab Complete!

Congratulations! You have successfully built a secure AWS network architecture featuring:

- ✅ **Secure SSH access** through a bastion host
- ✅ **Private EC2 instance** with no direct internet access
- ✅ **Internet connectivity** for private instances via NAT Gateway
- ✅ **Security group-based access controls**
- ✅ **Custom VPC** with proper subnet segmentation
- ✅ **Proper routing** for both public and private traffic

This architecture follows AWS security best practices and provides a solid foundation for more complex multi-tier applications.

---

## Next Steps

To further enhance this lab, consider:

1. **Adding Application Load Balancer** for web applications
2. **Implementing AWS Systems Manager Session Manager** for SSH-less access
3. **Adding RDS instances** in private subnets
4. **Implementing VPC Flow Logs** for network monitoring
5. **Adding CloudWatch monitoring** and alerting
6. **Implementing AWS Config** for compliance monitoring
