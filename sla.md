# SmartSolve Service Portal – Service Level Agreement (SLA)

**Author:** Samuel Jesse  
**Service:** SmartSolve Service Portal  
**Version:** 0.1 (Draft)  
**Last Updated:** (today’s date)

---

## 1. Service Purpose

The SmartSolve Service Portal provides customers with a platform to raise incidents and service requests, view their status, and track resolution progress.  
This SLA defines the expected service levels for availability, performance, and support response times.

---

## 2. Availability SLA

| Component | Target Availability | AWS Service Used | Notes |
|----------|---------------------|------------------|-------|
| Web Tier (Portal Access via ALB) | **99.9% per month** | ALB + EC2 | Requires at least 2 AZs |
| Application Tier | **99.9% per month** | EC2 | Health checks monitored by ALB |
| Database Tier | **99.95% per month** | RDS | Multi-AZ optional (we stay Single-AZ for Free Tier) |

---

## 3. Incident Priority Levels

| Priority | Definition | Target Response Time | Target Resolution Time |
|----------|-------------|-----------------------|-------------------------|
| **P1 – Critical** | Portal down, customers unable to raise tickets | 15 minutes | 4 hours |
| **P2 – High** | Major functionality degraded | 30 minutes | 8 hours |
| **P3 – Medium** | Minor issues, degraded performance | 4 hours | 2 business days |
| **P4 – Low** | Cosmetic issues, no impact on operations | 1 business day | 5 business days |

---

## 4. SLA → AWS Metrics Mapping

This section maps ITIL SLAs to AWS operational metrics using CloudWatch.

### **4.1 Availability Metrics**

| SLA Target | AWS Metric | Service | What It Means |
|------------|------------|----------|---------------|
| 99.9% Web Availability | `HealthyHostCount` | ALB | Number of healthy EC2 instances in each AZ |
| Portal Uptime | `HTTPCode_ELB_5XX` | ALB | High 5XX errors reduce availability |
| Server Availability | `StatusCheckFailed` | EC2 | Indicates reboot/terminate requirement |
| Database Uptime | `DatabaseConnections` and `FreeStorageSpace` | RDS | Shows whether DB is reachable |

---

### **4.2 Performance Metrics**

| Requirement | Metric | Notes |
|------------|--------|------|
| Page Load Below 2 Seconds | `TargetResponseTime` (ALB) | Measures latency to backend |
| Database Response | `ReadLatency` / `WriteLatency` | RDS | 

---

### **4.3 Capacity Metrics**

| Resource | CloudWatch Metric | IAM Responsibility |
|----------|-------------------|---------------------|
| CPU Utilization | `CPUUtilization` | EC2 | Auto Scaling trigger threshold ~70% |
| Memory (via CW Agent later) | `mem_used_percent` | EC2 | Requires agent installation |
| Network Throughput | `NetworkIn/Out` | EC2 | For load patterns |

---

## 5. Monitoring & Alerting Strategy

| Event | Threshold | AWS Alarm Action |
|-------|-----------|-------------------|
| EC2 CPU > 80% for 5 min | Performance Risk | CloudWatch Alarm → Email |
| ALB 5XX errors > 20/min | Service Degradation | Alarm + Log review |
| RDS storage < 20% | Data loss risk | Auto-Scaling storage (if enabled) |
| EC2 Status Check Failure | Instance unhealthy | Automatic replacement / manual action |

---

## 6. Reporting

Monthly SLA report will include:

- Availability %
- Incident summary (P1/P2/P3 volume)
- Top 3 improvements for next month
- Cost summary (target below $25)

---

## 7. Continual Improvement

All improvement ideas will be stored in `/docs/continual-improvement-log.md`.

