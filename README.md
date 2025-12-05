# SmartSolve Service Portal – ITIL & AWS SLA-Aligned 3-Tier App

**Author:** Samuel Jesse  
**Goal:** Build a realistic cloud portfolio project that maps **ITIL 4 practices** to an **AWS 3-tier architecture**, with clear SLAs and operational monitoring.

## Business Context

SmartSolve IT is a fictional managed services provider that offers IT support to small businesses.  
This project implements a small **IT support portal** where users can:

- Raise incidents and service requests
- Track ticket status and priority (P1, P2, P3)
- View basic SLA information (target response and resolution times)

The application is designed and operated using **ITIL 4 practices** and deployed on **AWS** within Free Tier / low-cost limits.

## Architecture 
## High-Level Architecture Diagram

  **What I implemented:**
  
 I set up the VPC, public subnets for the ALB and EC2 instances, and private subnets for the RDS database, with security groups enforcing strict least-privilege access between all tiers.

The visual diagram representation highlights:


**-Green:** authorised requests

**-Red:** malicious traffic blocked at the VPC

**-Orange:** valid users denied at the RDS security group

**-Pulsing green:** health checks


**I have attached a video below:**

https://github.com/user-attachments/assets/2032ebe5-35cc-47d2-87ff-2ba61d2c0006



  **What I implemented:**

I designed a visual network architecture diagram for the 3-tier application representing:

**-VPC**

**-subnets**

**-routing**

**-security groups**
<img width="840" height="431" alt="Smartsolve" src="https://github.com/user-attachments/assets/78196b97-0d95-48bc-9e22-ec38e02c8459" />



- **Networking**
  - Amazon VPC, public & private subnets
  - Application Load Balancer (ALB)
  - Route 53 for DNS (public or private hosted zone)
 
      **What I implemented:**

  **In the ap-southeast-2 (Sydney) region I**
    
• Created a dedicated VPC (10.0.0.0/16)

• Designed public and private subnets across two Availability Zones

• Attached an Internet Gateway and configured a public route table (0.0.0.0/0 → IGW)

• Kept the database tier in private subnets with no direct internet route

<img width="938" height="507" alt="VPC Resource Map" src="https://github.com/user-attachments/assets/bf44ccc2-619d-4784-ac06-84a4d43a2098" />


- **Compute**
  - EC2 instances for the web & application tier
  - ECS for a containerised microservice (later)
  - EKS (demo cluster only, for portfolio)
  - Lambda functions for automation & reporting

  **What I implemented:**

  Deployed an Application Load Balancer (ALB) across two Availability Zones in ap-southeast-2 (Sydney)

Created a target group with two EC2 instances (WebA & WebB) running in separate public subnets

Configured HTTP health checks on / so the ALB only routes traffic to healthy nodes

Tightened security by:

Exposing only the ALB to the internet

Restricting EC2 instances to accept HTTP only from the ALB security group

<img width="944" height="520" alt="step6 3-targets-healthy" src="https://github.com/user-attachments/assets/d37e7602-62c7-4284-a199-4f5229bc810a" />

<img width="945" height="516" alt="Create load balancer" src="https://github.com/user-attachments/assets/42697d70-660e-4557-a9e4-6ff36b173dac" />



**I have attached a video showing this:**

Browser showing the ALB URL in the address bar and the page content.

**Sometimes “SmartSolve Web Tier Running – Server A”**

**Sometimes “SmartSolve Web Tier Running – Server B”**

This proves:

ALB is distributing traffic across AZs

Health checks are working

SLA availability design is real (multi-AZ)

https://github.com/user-attachments/assets/20f47a43-c90b-4119-9044-ac8a44acefb8


- **Storage & Data**
  - S3 for static assets and logs
  - RDS (MySQL/PostgreSQL) for tickets, users, and SLA data

- **Operations & Governance**
  - CloudWatch metrics, logs, dashboards, alarms
  - IAM roles and policies following least privilege
  - Billing alarms to keep monthly cost under
 
      **What I implemented:**


-Created an IAM Role for EC2 with:
AmazonSSMManagedInstanceCore for secure keyless management
CloudWatchAgentServerPolicy to support monitoring and sla metrics, this ensures all administrative access is logged and auditable.

-Launched EC2 instances in ap-southeast-2a and ap-southeast-2b

-Enabled SSM for secure, keyless access (no ssh keys needed)

-Installed a lightweight web application via systems manager Run Command

-Configured security groups following least-privilege principles

-Prepared the environment for the application load balancer that will enforce health checks and sla standards

**I have attached a video below:**


https://github.com/user-attachments/assets/0006ad9f-3198-499d-a014-84d5e37b3d5b



<img width="1920" height="1200" alt="step5 1-iam-role png" src="https://github.com/user-attachments/assets/57102f42-1630-428d-b8b6-4babd11c6717" />


## ITIL 4 Practices Demonstrated

- Service level management
- Service design & architecture management
- Change enablement (GitHub-based)
- Incident & problem management (simulated scenarios)
- Monitoring & event management (CloudWatch)
- Information security management (IAM & security groups)
- Continual improvement (Improvement log in `/docs`)

## How SLAs Map to AWS Services

This project demonstrates how ITIL 4 Service Level Management translates into AWS architecture:

- **Availability SLA (99.9%) →**  
  Achieved using ALB health checks, multi-AZ EC2 deployment, and CloudWatch uptime metrics.

- **Performance SLA →**  
  Measured with ALB TargetResponseTime and RDS read/write latency.

- **Incident Response SLA →**  
  CloudWatch alarms notify when thresholds are breached (CPU, 5XX errors, RDS free storage).

- **Capacity & Scalability →**  
  Auto Scaling (later) ensures capacity meets demand while staying within SLA.

- **Security SLA →**  
  IAM least privilege, Security Group controls, CloudTrail logs.

  ## Repository Structure (Planned)

- `/docs` – ITIL documents (Service description, SLA, runbooks, improvement log)
- `/infra` – Infrastructure definitions (manual steps at first, later IaC)
- `/app` – Application code (web portal, APIs)
- `/monitoring` – Dashboards, alerts, and operational scripts

  > **Status:** Just started. Repo currently contains the initial service description and plan. Infrastructure and code will be added step by step.
