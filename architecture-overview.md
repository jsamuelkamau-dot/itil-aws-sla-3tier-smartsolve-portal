# SmartSolve Service Portal – Architecture Overview

**Author:** Samuel Jesse  
**Version:** 0.1  
**Last Updated:** 27/11/2025 

---

## 1. Design Drivers (ITIL 4 Service Design)

This architecture is designed using ITIL 4 Service Design and Service Level Management principles:

- **Availability:** Target 99.9% availability for the web and application tiers.
- **Security:** Use least privilege (IAM), security groups, and private subnets for the database.
- **Cost:** Stay within AWS Free Tier / under $25 per month (no NAT Gateway at this stage).
- **Operability:** Use CloudWatch for monitoring, logs, and alerts aligned to SLAs.
- **Scalability:** Design for future Auto Scaling and containerisation (ECS/EKS) without redesigning the network.

---

## 2. AWS Region

- **Region:** `ap-southeast-2` (Asia Pacific – Sydney)

Reasons:
- Close to the (assumed) user base.
- Good latency from Adelaide.
- Common region for AU workloads.

---

## 3. VPC & Subnets

- **VPC CIDR:** `10.0.0.0/16`

### 3.1 Subnet Plan

We use two Availability Zones (AZ A and AZ B). Exact AZ codes (e.g. `ap-southeast-2a`, `2b`) will be chosen in AWS.

**Public Subnets (Web/App Tier + ALB):**

- `10.0.1.0/24` – Public Subnet A (ALB + EC2)
- `10.0.2.0/24` – Public Subnet B (ALB + EC2)

**Private Subnets (Database Tier):**

- `10.0.11.0/24` – Private DB Subnet A (RDS)
- `10.0.12.0/24` – Private DB Subnet B (for future Multi-AZ)

### 3.2 Routing

- **Internet Gateway (IGW)** attached to the VPC.
- **Public Route Table:**
  - Associated with `10.0.1.0/24` and `10.0.2.0/24`
  - Route: `0.0.0.0/0 → IGW`
- **Private Route Table:**
  - Associated with `10.0.11.0/24` and `10.0.12.0/24`
  - No direct internet route (no NAT Gateway to control cost).
  - RDS remains internal-only.

> **ITIL Note:** This design balances **availability and security** while respecting a cost constraint (no NAT Gateway). Internet exposure is limited to the ALB and EC2 instances in public subnets.

---

## 4. 3-Tier Application Layout

### 4.1 Presentation Tier (Web Tier)

- **AWS Services:**
  - Application Load Balancer (ALB)
  - EC2 instances (initially simple web/app server)
- **Location:**
  - ALB in both Public Subnet A & B.
  - EC2 instances in both Public Subnet A & B.

### 4.2 Application Tier

For simplicity and cost reasons, the **application tier runs on the same EC2 instances as the web tier** initially. Later, we may split into:

- Web tier (public)  
- App tier (private behind ALB using private IPs only)

### 4.3 Data Tier

- **RDS** (MySQL or PostgreSQL) in private subnets (`10.0.11.0/24` and later `10.0.12.0/24`).
- Not publicly accessible.
- Only the EC2 security group is allowed to connect to RDS on the database port.

---

## 5. Security Groups (High Level)

- **ALB-SG**
  - Inbound: HTTP/HTTPS (`80`/`443`) from the internet (`0.0.0.0/0`)
  - Outbound: To EC2-SG on HTTP (`80`)

- **EC2-SG**
  - Inbound: HTTP (`80`) from ALB-SG only
  - (Later) SSM Session Manager for management instead of SSH
  - Outbound: To RDS-SG on DB port

- **RDS-SG**
  - Inbound: DB port (`3306` MySQL or `5432` PostgreSQL) from EC2-SG only
  - No public inbound traffic

> **ITIL Note:** This supports **Information Security Management** by enforcing least privilege and zoning between web, app, and data.

---

## 6. Operations & Monitoring

- **CloudWatch Metrics**
  - EC2: CPUUtilization, StatusCheckFailed
  - ALB: HealthyHostCount, TargetResponseTime, HTTPCode_ELB_5XX
  - RDS: CPUUtilization, FreeStorageSpace, ReadLatency, WriteLatency

- **CloudWatch Alarms (planned)**
  - High CPU on EC2
  - ALB 5XX errors
  - Low RDS storage

> These metrics link back to the SLAs defined in `/docs/sla.md`.

---

## 7. Future Enhancements

- Move application logic into **ECS** (containers) behind the same ALB.
- Spin up a small **EKS** cluster (for demo only, limited runtime to control cost).
- Introduce **Lambda** functions for automated health checks and SLA reporting.
- Add WAF for additional protection on the ALB.


## Load Balancer & High Availability (Completed)

- Application Load Balancer (ALB-SmartSolve-Web) deployed across ap-southeast-2a and ap-southeast-2b
- Target group TG-SmartSolve-Web with EC2-WebA and EC2-WebB
- Health checks on `/` used to measure availability SLA
- ALB-SG exposes HTTP (80) to the internet
- EC2-SG now only allows HTTP from ALB-SG (no direct internet access)

- **Create load balancer**
- 
<img width="945" height="516" alt="Create load balancer" src="https://github.com/user-attachments/assets/6eb3c3c3-9312-4174-a5fd-cc38ca5cefab" />

**step6.2-target-group**

<img width="952" height="516" alt="step6 2-target-group" src="https://github.com/user-attachments/assets/e312fc85-9438-4b0f-9590-6d964b8f6f67" />

**step6.4-alb-browser**

<img width="959" height="383" alt="step6 4-alb-browser" src="https://github.com/user-attachments/assets/f8abe057-7fd6-4442-9dfe-3af07f687337" />

