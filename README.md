# aws-NAT-gateway
# AWS NAT Gateway with CloudFormation

This project demonstrates how to provide **private internet access** for EC2 instances using a **NAT Gateway** in AWS.  
The architecture was deployed using a **CloudFormation template** (provided by Adrian Cantrill) and enhanced with manual configuration of the NAT Gateway and routing.
# Implementation Steps: Private Internet Access with NAT Gateway

This document details the steps I followed to deploy a secure VPC architecture using **CloudFormation** and configure **private internet access** with a NAT Gateway.


-Design
![design](/vpcdesign.jpeg)
 
---

## 1. Deploy VPC with CloudFormation
- Used **Adrian Cantrill’s CloudFormation template** to create the base VPC.
- The template provisioned:
  - VPC with CIDR block `10.16.0.0/16`
  - Public and private subnets across multiple Availability Zones
  - Internet Gateway
  - Route tables (basic)

✅ Screenshot: *CloudFormation Stack Created*  

---

## 2. Configure NAT Gateway
- Navigated to **VPC > NAT Gateways** in the AWS Console.
- Created a NAT Gateway in a **public subnet**.
- Attached an **Elastic IP** to allow outbound internet access.
- Ensured high availability by placing NAT Gateway in each AZ (best practice).

✅ Screenshot: *NAT Gateway Details*  

---

## 3. Update Route Tables
- Edited the **private subnet route tables**:
  - Added route `0.0.0.0/0` → Target: **NAT Gateway**
- Confirmed **public subnets** were routing to the Internet Gateway.
- This ensures:
  - Instances in private subnets → outbound via NAT
  - Instances in public subnets → direct internet access

✅ Screenshot: *Route Table Configurations*  

---

## 4. Launch EC2 Instance in Private Subnet
- Created a **t2.micro** instance in one of the **App private subnets**.
- Associated it with a **Security Group** allowing outbound traffic.
- Connected to the instance via a **Bastion Host** in the public subnet (SSH).  

✅ Screenshot: *App Subnet Instance Running*  

---

## 5. Test Internet Access from Private Instance
- Logged into the private EC2 instance using the Bastion Host.
- Verified outbound internet access by running:
  ```bash
  ping 8.8.8.8 -c 4
  curl https://www.google.com
  sudo yum update -y   # (Amazon Linux)

