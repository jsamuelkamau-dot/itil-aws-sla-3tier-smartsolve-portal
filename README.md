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

I designed a visual network architecture diagram for the 3-tier application representing:

**-VPC**

**-subnets**

**-routing**

**-security groups**

<img width="840" height="431" alt="Smartsolve" src="https://github.com/user-attachments/assets/78196b97-0d95-48bc-9e22-ec38e02c8459" />

**Scurity in depth visual diagram**

  **What I implemented:**
  
 I set up the VPC, public subnets for the ALB and EC2 instances, and private subnets for the RDS database, with security groups enforcing strict least-privilege access between all tiers.

The visual diagram representation highlights:


**-Green:** authorised requests

**-Red:** malicious traffic blocked at the VPC

**-Orange:** valid users denied at the RDS security group

**-Pulsing green:** health checks


**I have attached a video below:**

https://github.com/user-attachments/assets/2032ebe5-35cc-47d2-87ff-2ba61d2c0006


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

**Attached is the VPC screenshot**

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


**Attached is the screenshot and video**

**HEALTHY TARGETS SCREENSHOT**

<img width="944" height="520" alt="step6 3-targets-healthy" src="https://github.com/user-attachments/assets/d37e7602-62c7-4284-a199-4f5229bc810a" />

**CREATED LOAD BALANCER SCREENSHOT**

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

**I have attached a video below and screenshot:**


https://github.com/user-attachments/assets/0006ad9f-3198-499d-a014-84d5e37b3d5b


**EC2 RUNNING SCREENSHOT**

<img width="934" height="518" alt="step5 3-ec2-running" src="https://github.com/user-attachments/assets/1d12bab8-6d21-4528-b3f5-b4064f70c5f0" />

**SYSTEM MANAGER SSM SCREENSHOT**

<img width="951" height="490" alt="System manager SSM" src="https://github.com/user-attachments/assets/24f025d4-a5bf-47ef-8185-e3174f84fbfd" />

**SECURITY GROUPS SCREENSHOT**

<img width="947" height="482" alt="SECURITY Groups" src="https://github.com/user-attachments/assets/016cd373-1472-433c-af33-c64887adfd59" />


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


## SLA Monitoring & ITIL Event Management 

- Enabled detailed monitoring (1-minute metrics) on EC2 instances
- Created CloudWatch alarms for:
  - Application availability (HealthyHostCount < 2)
  - Performance SLA (TargetResponseTime ≥ threshold)
  - CPU Utilization for capacity management
- Built CloudWatch dashboard showing key service metrics
- SNS alerts notify for SLA breaches / incidents


<img width="910" height="512" alt="AWS SNS Email confirmation" src="https://github.com/user-attachments/assets/e3785843-88c8-4e4f-a950-aa952c479449" />


<img width="956" height="516" alt="step7 2-availability-alarm" src="https://github.com/user-attachments/assets/13d47298-c874-4e89-852c-41770ccfdb48" />

<img width="950" height="518" alt="step7 2-cpu-alarm" src="https://github.com/user-attachments/assets/09255cfa-b0a4-4d52-8ddc-54bb791662c8" />

##  Database Tier (RDS MySQL)

- Created an RDS subnet group using private subnets in ap-southeast-2a and ap-southeast-2b
- Added RDS-SG security group allowing MySQL (3306) only from EC2-SG (web tier)
- Deployed Amazon RDS MySQL (smartsolve-db) in private subnets (no public access)
- Verified connectivity from EC2-WebA using the internal RDS endpoint
- Created application database `smartsolve_portal` and dedicated DB user `smartsolve_app`

<img width="959" height="520" alt="step8 5-rds-connection-and-db" src="https://github.com/user-attachments/assets/4bc936d7-b80a-44e9-93cf-972ede777fce" />

<img width="950" height="515" alt="step8 3-rds-creating" src="https://github.com/user-attachments/assets/cf78c5fd-ded5-47d2-b0a8-eca05f8fe2c9" />

**DATABASE CREATION**



https://github.com/user-attachments/assets/85c3359a-728e-4837-8af8-3b6789890b19

## – Application Tier Connected to RDS (3-Tier Integration)

- Installed PHP and MySQL client libraries on EC2-WebA and EC2-WebB
- Created db-test.php to validate connectivity to the RDS MySQL database
- Successfully connected from both EC2 instances using internal RDS endpoint
- Verified full 3-tier communication through ALB → EC2 → RDS
- Demonstrated high availability and load balancing at the application tier

<img width="647" height="398" alt="step9 3-db-connected-webA" src="https://github.com/user-attachments/assets/72041bef-a14f-441f-a2bc-40be9e301c35" />

<img width="818" height="367" alt="image" src="https://github.com/user-attachments/assets/292c4e25-6077-446d-8578-d78bbfbf2a10" />

**Database connection Test Through the ALB(vedio attached)**





https://github.com/user-attachments/assets/9258de1c-e96b-44a9-916a-c626b6beacf0

## – SmartSolve Application Layer

- Deployed a simple PHP-based SmartSolve Service Portal on both EC2-WebA and EC2-WebB
- Application connects to the RDS MySQL database using a shared db.php include
- Home page displays which web server handled the request plus live database list
- Verified full 3-tier path through the Application Load Balancer (ALB → EC2 → RDS)
<img width="364" height="224" alt="Application Layer" src="https://github.com/user-attachments/assets/b857b6dc-edb1-47f6-b934-c4e3ff1cea3a" />



# SmartSolve Ticketing Portal – Database Schema 

Database: `smartsolve_portal`

## Tables

- `users`  
  Stores portal users (admins, agents, and later customers). Includes role, status, and timestamps.

- `ticket_categories`  
  Classification of tickets (Incident, Request, Billing, Access Issue, etc.).

- `problems`  
  ITIL Problem Management / Known Error records. Root cause, workaround, permanent fix.

- `tickets`  
  Core ticket records:
  - Customer details (name, email)
  - Subject and description
  - Category and priority
  - Status workflow: New → In Progress → Waiting on Customer → Resolved → Closed
  - SLA field (`sla_due_at`)
  - Link to `problems` for known errors
  - FULLTEXT index (`ft_tickets_subject_desc`) for similarity matching

- `ticket_comments`  
  Conversation history and internal notes for each ticket.

- `ticket_status_history`  
  Audit trail of all status changes, who changed them, and when.

- `ticket_attachments`  
  Metadata for files attached to tickets (stored in S3 later).

## ITIL Mapping

- Incident & Request Management → `tickets`, `ticket_categories`
- Problem Management → `problems`, `tickets.problem_id`
- Service Level Management → `priority`, `sla_due_at`
- Knowledge / Known Errors → problem records tied to similar tickets
- Auditability → `ticket_status_history`, `ticket_comments`

**– Create users table (for login & roles)**
<img width="946" height="514" alt="11 2 – Create users table (for login   roles)" src="https://github.com/user-attachments/assets/d0dbc290-5ea1-42f0-868d-684acb3c2860" />

**– Create ticket_categories table**
<img width="956" height="517" alt="11 3 – Create ticket_categories table" src="https://github.com/user-attachments/assets/2e322129-aa0a-4b44-820c-2f5d361b4430" />

**Create tickets table (core ticket info + SLA + problem link)**

<img width="953" height="513" alt="11 5 – Create tickets table (core ticket info + SLA + problem link)" src="https://github.com/user-attachments/assets/ade16a25-493b-4d21-9567-fbfdae200985" />

**– Create ticket_status_history table (audit trail)**

<img width="950" height="520" alt="11 8 – Create ticket_status_history table (audit trail)" src="https://github.com/user-attachments/assets/3c3e166f-0daa-468e-a7e3-44f4637f6707" />

**SHOW TABLES**
<img width="958" height="517" alt="SHOW TABLES" src="https://github.com/user-attachments/assets/2aef9a97-309d-4070-981c-2d9c1fdb43e0" />




## 🎫 SmartSolve Service Portal — Ticketing System Live on AWS

successfully built and deployed a custom ITIL-aligned service portal on AWS using a highly available 3-tier architecture.
feat: working ticket creation with SLA calculation and similarity matching

### 🚀 Key Features Implemented
- Ticket creation via web UI behind an Application Load Balancer
- SLA calculation based on ticket priority (Low → Critical)
- MySQL RDS (private subnet) with strict security group isolation
- FULLTEXT similarity matching to detect related incidents automatically
- Known Error / Problem linkage to prevent repeat work
- Real-time server identification (ALB → EC2 WebA/WebB)
- Secure architecture (no direct DB access from the internet)

## New-ticket-form

<img width="944" height="545" alt="new-ticket-form" src="https://github.com/user-attachments/assets/695cf9ad-d34d-42f4-bc73-2c7fe2c6788d" />

## Ticket-created-similarity

<img width="944" height="557" alt="ticket-created-similarity" src="https://github.com/user-attachments/assets/d640e686-d122-40bf-ac5e-c8bc6d97bc71" />

## Ticket created successfully via ALB!



https://github.com/user-attachments/assets/0490cba2-1e4c-4356-bbba-ce440d8238b2



### 🏗 Architecture Overview
- **Web Tier:** EC2 instances across multiple AZs behind ALB
- **App Logic:** PHP (prepared statements, SLA logic, similarity scoring)
- **Data Tier:** Amazon RDS (MySQL) in private subnets
- **Security:** Least privilege SGs, IAM roles, no SSH keys (SSM)

### 🧩 Why This Project Stands Out
Unlike basic CRUD demos, this project demonstrates:
- ITIL practices (Incident, SLA, Known Error)
- Enterprise problem-reduction logic via similarity matching
- Real AWS networking, security, and availability design
- Production-style troubleshooting and iteration

-----
📌**Project Closure & Outcomes**
Project Status: Completed (Intentional Closure)

This project has been intentionally concluded after successfully achieving its original and extended objectives.

The SmartSolve Service Portal was designed as a real-world, SLA-aware IT service platform to bridge theory (ITIL 4 + AWS architecture) with hands-on operational experience. Over the course of the build, the project evolved far beyond an academic exercise and became a genuine simulation of enterprise IT service delivery.

------
🎯 **Key Objectives Achieved**

✅ Real-World AWS Architecture Experience

Designed and implemented a production-style AWS 3-tier architecture

Deployed across:

VPC with public and private subnets

Application Load Balancer

EC2 web tier

Amazon RDS (MySQL) in private subnets

Applied security best practices:

Least-privilege security groups

No public database exposure

Controlled ingress/egress rules

Diagnosed and resolved real infrastructure issues (routing, security groups, ALB health checks, IAM, SSM, PHP-MySQL compatibility)

The project was deliberately broken and rebuilt multiple times to simulate real production troubleshooting — not just “happy-path” deployment.

✅ ITIL-Aligned Service Management in Practice

Implemented an ITIL-style incident lifecycle:

New → In Progress → Waiting on Customer → Resolved → Closed

Embedded SLA logic directly into the application:

SLA due dates calculated automatically based on ticket priority

Built an agent dashboard reflecting real service desk workflows:

Status updates

Assignment

SLA visibility

Audit timestamps
--------

✅ Intelligent Ticket Similarity Matching (Stand-Out Feature)

One of the most valuable outcomes of this project was the successful implementation of automatic related-ticket matching, a feature commonly found in enterprise ITSM tools.

When a new ticket is raised, the system:

Analyzes historical tickets

Identifies likely related incidents based on subject and description similarity

Surfaces previous resolutions and patterns

Helps prevent duplicated effort and speeds up incident resolution

This feature directly addresses a real operational pain point in IT support environments and demonstrates applied problem-solving beyond basic CRUD applications.

------

✅ Certification Outcomes (Primary Success Metric)

The practical depth gained from this project directly contributed to achieving the following certifications:

🏅 AWS Certified Cloud Practitioner – 2025

🏅 AWS Certified Solutions Architect – Associate (SAA) – 2026

Hands-on experience with:

VPC design

Load balancing

High availability

Security boundaries

Fault isolation
made AWS exam concepts intuitive rather than theoretical.

------
🧠 Key Learnings

Real AWS environments rarely fail for one reason — issues often span networking, security, IAM, and application layers

Documentation, step-by-step validation, and rollback thinking are critical

SLA awareness must be designed into systems, not added later

Enterprise value comes from operational intelligence, not just infrastructure

-------
🛑 Why the Project Was Stopped

The project was intentionally paused after:

All learning objectives were met

Core architectural goals were achieved

Advanced functionality (ticket similarity matching) was proven

Certification outcomes were successfully reached

At this stage, continuing would shift the project from a learning portfolio into a product build, which was beyond the original scope.

------
📂 Repository Purpose Going Forward

This repository remains as:

A documented engineering journey

Evidence of real-world AWS + ITIL capability

A reference architecture for future projects

A foundation that can be extended if needed

--------
✍️ Designed & Developed by Samuel Jesse

SmartSolve Service Portal
ITIL-aligned • AWS 3-Tier Architecture • SLA-aware workflows • Operational intelligence
