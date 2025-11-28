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

<img width="537" height="488" alt="Screenshot 2025-11-27 234439" src="https://github.com/user-attachments/assets/23990a27-cd07-48b3-9db6-c1fa6f6c943f" />

<img width="840" height="431" alt="Smartsolve" src="https://github.com/user-attachments/assets/78196b97-0d95-48bc-9e22-ec38e02c8459" />



- **Networking**
  - Amazon VPC, public & private subnets
  - Application Load Balancer (ALB)
  - Route 53 for DNS (public or private hosted zone)

- **Compute**
  - EC2 instances for the web & application tier
  - ECS for a containerised microservice (later)
  - EKS (demo cluster only, for portfolio)
  - Lambda functions for automation & reporting

- **Storage & Data**
  - S3 for static assets and logs
  - RDS (MySQL/PostgreSQL) for tickets, users, and SLA data

- **Operations & Governance**
  - CloudWatch metrics, logs, dashboards, alarms
  - IAM roles and policies following least privilege
  - Billing alarms to keep monthly cost under **$25**

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
