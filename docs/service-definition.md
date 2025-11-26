# SmartSolve Service Portal – Service Definition

## 1. Service Overview

- **Service name:** SmartSolve Service Portal
- **Service owner:** Samuel Jesse
- **Service type:** IT Support / Service Desk web portal
- **Customers:** Small and medium businesses using SmartSolve IT managed services
- **Users:** End-users raising incidents and service requests

## 2. Business Outcomes

- Faster and more transparent incident and request handling
- Clear communication of SLAs (targets for response and resolution)
- Improved visibility of service health for both IT and business stakeholders

## 3. ITIL 4 Practices in Scope

- Service level management
- Incident management
- Change enablement
- Monitoring & event management
- Problem management
- Information security management
- Continual improvement

## 4. High-Level SLAs (Draft)

> These are initial targets. They will be refined later in the project.

- **Availability:** 99.9% monthly
- **P1 Incident (Critical):**
  - Response time: 15 minutes
  - Target resolution: 4 hours
- **P2 Incident (High):**
  - Response time: 30 minutes
  - Target resolution: 8 hours
- **P3 Incident (Normal):**
  - Response time: 4 hours
  - Target resolution: 2 business days

## 5. Service Hours & Support Model

- Service hours: 24x7 for portal access
- Support coverage: 8x5 (business hours) for non-P1 incidents, 24x7 for P1

## 6. Dependencies (Planned)

- AWS region (e.g., ap-southeast-2)
- VPC with public & private subnets
- ALB, EC2, RDS, S3, Route 53
- CloudWatch, IAM

## 7. Continual Improvement

- Improvement ideas and actions will be tracked in `/docs/continual-improvement-log.md`.
