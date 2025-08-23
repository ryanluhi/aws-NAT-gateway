# aws-NAT-gateway

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

-Cloud Formation
![CloudFormation](/cloudformation.jpeg) 

-Stack
![Stack Created](/stack.jpeg)

---

## 2. Configure NAT Gateway
- Navigated to **VPC > NAT Gateways** in the AWS Console.
- Created a NAT Gateway in a **public subnet**.
- Attached an **Elastic IP** to allow outbound internet access.
- Ensured high availability by placing NAT Gateway in each AZ (best practice).

![NAT Gateway Details](/natgw.jpeg) 

---

## 3. Create and Update Route Tables
- Edited the **private subnet route tables**:
  - Added route `0.0.0.0/0` → Target: **NAT Gateway**
- Confirmed **public subnets** were routing to the Internet Gateway.
- This ensures:
  - Instances in private subnets → outbound via NAT
  - Instances in public subnets → direct internet access
-Create a Route Table
![create a rt](/routetable.jpeg)

-Route Table Configuration
![Route Table Configurations](/rtprivateA.jpeg)
![Route Table Configurations](/rtprivateB.jpeg)
![Route Table Configurations](/rtprivateC.jpeg)

---

## 4. Launch EC2 Instance in Private Subnet
 
 ![App Subnet Instance Running](/testinstance.jpeg) 

---

## 5. Test Internet Access from Private Instance
- Logged into the private EC2 instance .
- Verified outbound internet access by running:
![connect to the instance](/testinstance1.jpeg)
   ping 1.1.1.1
  ![test](/testinstance2.jpeg)
