# Infrastructure, Operations & Systems Engineering Portfolio

> Technical Operations portfolio focused on infrastructure modernization, reliability engineering, observability, automation, and production system stability across cloud and on-prem environments.

---

## 👤 Author

**Dennis Lubins**  
Technical Operations Engineer

---

# 📌 Overview

This repository showcases real-world infrastructure, operations, and reliability engineering case studies built from hands-on experience designing, stabilizing, monitoring, and modernizing production systems.

The focus of this portfolio is not isolated technologies, but the operational systems and engineering approaches used to improve:

- Reliability
- Visibility
- Performance
- Automation
- Operational consistency
- Incident response
- Infrastructure sustainability

While SQL Server appears throughout several projects, it is presented as part of broader operational and platform engineering environments rather than as a standalone specialization.

---

# 🧠 Operational Philosophy

→ [How I Approach Production Systems](./docs/00-operational-philosophy.md)

This document outlines the operational mindset used throughout the projects in this portfolio:

- Visibility → Understanding → Targeted Fixes → Monitoring
- Systems thinking over reactive troubleshooting
- Operationalization of solutions for long-term stability

---

# 🚀 Featured Case Studies

## 🏗️ Infrastructure & Platform Engineering

### 🧱 [Building an End-to-End Infrastructure, Deployment & Observability Lab on AWS](./docs/case-studies/infrastructure/aws-observability-lab.md)

Designed and implemented a full infrastructure → deployment → monitoring → automation workflow using:

- Terraform
- Ansible
- Docker
- SQL Server
- Prometheus
- Grafana

#### Highlights
- Automated infrastructure provisioning
- Configuration management with Ansible
- Real-time observability pipeline
- Automated SQL Server recovery workflow
- Integrated multi-container workload deployment

#### Related Lab Phases
- [Phase 1 – Control Plane Setup](./docs/case-studies/infrastructure-lab/phase-01-control-plane-setup.md)
- [Phase 2 – Infrastructure Provisioning](./docs/case-studies/infrastructure-lab/phase-02-terraform-infrastructure.md)
- [Phase 3 – Configuration Management](./docs/case-studies/infrastructure-lab/phase-03-ansible-configuration.md)
- [Phase 4 – Workload, Monitoring & Automation](./docs/case-studies/infrastructure-lab/phase-04-workload-monitoring-automation.md)

---

### 💾 [AWS S3 Backup Architecture & Lifecycle Management](./docs/case-studies/infrastructure/aws-s3-backup-architecture-lifecycle-management.md)

Designed and implemented a secure, cost-optimized AWS-based backup architecture supporting long-term archival retention, lifecycle management, operational monitoring, and Linux-native automation workflows.

#### Highlights
- Multi-tier S3 + Glacier storage lifecycle strategy
- IAM least-privilege access controls
- AWS Budgets and Cost Anomaly Detection monitoring
- Linux-based rclone automation workflows
- Encrypted offsite backup architecture
- Operational governance and cost optimization design

---

### ⚙️ [Legacy Deployment Automation & SSRS Modernization](./docs/case-studies/infrastructure/legacy-deployment-modernization.md)

Modernized and automated a heavily manual enterprise deployment environment supporting web applications, Citrix-hosted client platforms, and SSRS reporting infrastructure.

#### Highlights
- Reduced deployment windows from ~90 minutes to ~15 minutes
- Engineered custom SSRS deployment automation framework
- Automated deployments using TeamCity, Octopus Deploy, and PowerShell
- Eliminated manual deployment processes across all environments
- Migrated legacy Windows Server 2008 / SSRS platforms to 2019

---

### 📋 [Enterprise Deployment Governance & Operational Standardization](./docs/case-studies/infrastructure/enterprise-deployment-governance.md)

Designed and operationalized a centralized enterprise deployment governance framework supporting SOC-compliant production change management, rollback coordination, deployment communication, and cross-functional operational standardization.

#### Highlights
- Centralized fragmented deployment procedures into a unified Confluence operational hub
- Standardized deployment governance and communication workflows
- Coordinated production deployment and rollback operations across multiple technical teams
- Developed scalable onboarding and operational training frameworks
- Automated deployment tracking and communication workflows using SQL-driven reporting integrations
- Trained and mentored engineers on enterprise deployment coordination processes

---

### 🖥️ [Enterprise Virtualization Migration & Infrastructure Modernization](./docs/case-studies/infrastructure/virtualization-modernization.md)

Designed and executed a phased migration from legacy hyperconverged infrastructure to a modernized server platform.

#### Highlights
- Infrastructure modernization strategy
- Phased workload migration
- Risk-reduction planning
- VMware operational management
- Legacy infrastructure retirement

---

### 🚀 [SQL Server Migration Strategy & Execution System](./docs/case-studies/infrastructure/sql-server-migration-strategy.md)

Developed a controlled SQL Server upgrade methodology separating engine-level changes from query-processing behavior changes.

#### Highlights
- Controlled SQL Server 2017 → 2022 upgrade strategy
- Query Store-based validation
- Incremental compatibility-level rollout
- Snapshot-based rollback planning
- Performance regression analysis

---

# 📡 Reliability Engineering & Observability

### 🔥 [Reducing 2,000+ Daily SQL Server Deadlocks to Near Zero](./docs/case-studies/reliability/deadlock-reduction.md)

Investigated and remediated systemic deadlock conditions across multiple production SQL Server environments.

#### Highlights
- Extended Events-based deadlock analysis
- Index and statistics optimization
- Workload conflict identification
- Monitoring-driven remediation strategy
- Long-term operational stabilization

#### Results
- Reduced ~2,000 daily deadlocks to near zero
- Significant reduction in application failures and operational noise

---

### ⚙️ [Eliminating TempDB Latency Caused by Inefficient Statistics Maintenance](./docs/case-studies/reliability/tempdb-maintenance-optimization.md)

Identified and resolved maintenance-driven TempDB contention caused by inefficient blanket statistics updates.

#### Highlights
- Workload-aware maintenance strategy
- Heavy-table prioritization
- Phased statistics maintenance model
- Reduced maintenance overhead
- Improved maintenance window stability

---

### 📊 [Diagnosing Burst-Induced SQL Server Disk Latency Without Increasing Infrastructure Cost](./docs/case-studies/reliability/burst-induced-disk-latency.md)

Performed targeted interval-based analysis of transient SQL Server disk latency events on Azure-hosted infrastructure.

#### Highlights
- Short-interval workload capture
- Azure metric correlation
- Checkpoint / dirty-page flush analysis
- Alert tuning and operational optimization
- Monitoring refinement without infrastructure expansion

---

### 📬 [Building a SQL Server Alerting System Without Database Mail](./docs/case-studies/reliability/alerting-pipeline-modernization.md)

Designed a queue-based alerting pipeline to replace unreliable SQL Server Database Mail workflows.

#### Highlights
- Decoupled alert processing architecture
- Queue-based lifecycle tracking
- PowerShell-driven delivery workflow
- Improved operational visibility
- Enhanced alert reliability and troubleshooting

---

### 🛡️ [Azure SQL Backup Observability & Operational Recovery Framework](./docs/case-studies/reliability/azure-sql-backup-observability-framework.md)

Designed and operationalized a multi-layer Azure SQL backup observability and response framework supporting production SQL Server workloads hosted in Azure.

#### Highlights
- Built Azure Backup + SQL monitoring operational framework
- Developed centralized SSRS backup observability dashboard
- Implemented cadence-based SQL monitoring and alerting
- Identified cross-server non-copy-only backup chain interference
- Discovered silent Database Mail notification failure
- Standardized Azure Backup incident response runbooks
- Separated alerting, monitoring, observability, and response responsibilities

---

# ☁️ Cloud Security & Automation

### 🛡️ [Cloud-Based Malware Detection & Secure File Processing Pipeline](./docs/case-studies/cloud-security/azure-malware-detection-pipeline.md)

Designed and implemented a cloud-native malware scanning and secure file-processing workflow on Azure.

#### Highlights
- Container-based malware scanning
- Serverless workflow integration
- Automated response pipeline
- Real-time detection and alerting
- Cloud platform operationalization

---

# 🛠️ Core Focus Areas

- Infrastructure Modernization
- Deployment Automation & CI/CD Operationalization
- Reliability Engineering
- Monitoring & Observability
- Incident Response & Root Cause Analysis
- Production Systems Engineering
- Cloud Operations & Hybrid Infrastructure
- Automation & Operational Workflows
- Storage & Data Protection Architecture
- Platform Stability & Performance
- Disaster Recovery & Controlled Change Management
- Deployment Governance & Operational Standardization
- Change Management & Release Coordination
- Knowledge Management & Operational Enablement
- Cross-Functional Technical Operations
- Backup Observability & Operational Resilience
- Operational Monitoring Architecture
- Reliability & Recovery Engineering

---

# ⚙️ Technologies & Platforms

## 🏗️ Infrastructure & Cloud
- AWS (EC2, S3, IAM, SNS, Budgets, Cost Anomaly Detection)
- Microsoft Azure
- VMware
- Dell PowerEdge
- Hyperconverged Infrastructure
- Windows Server
- Azure Recovery Services Vault

## 🤖 Automation & Deployment
- PowerShell
- Terraform
- Ansible
- TeamCity
- Octopus Deploy
- Confluence

## 📈 Monitoring & Observability
- Grafana
- Prometheus
- SQL Server Extended Events
- Azure Backup
- SQL Server Database Mail
- SQL Server Reporting Services (SSRS)

## 🗄️ Data & Reporting Platforms
- SQL Server
- SSRS
- Query Store

## 💾 Storage & Backup
- Amazon S3
- S3 Glacier Flexible Retrieval
- S3 Glacier Deep Archive
- rclone

## 📦 Workload & Containerization
- Docker
- Docker Compose
- Citrix
- Distrobox

## 🔧 Operational Practices
- Infrastructure Modernization
- CI/CD Operationalization
- Root Cause Analysis
- Incident Response
- Disaster Recovery Planning
- Deployment Governance
- Production Change Management
- Operational Documentation Systems
- Rollback Coordination
- Technical Training & Enablement
- Backup Observability
- Operational Runbook Engineering
- Incident Workflow Standardization

---

# 📂 Repository Structure

```text
docs/
├── 00-operational-philosophy.md
│
├── case-studies/
│   ├── infrastructure/
│   ├── reliability/
│   └── cloud-security/
│
├── infrastructure-lab/
│
└── images/
```

---

# ⚠️ Notes

- All environments and examples have been anonymized
- Documentation focuses on operational methodology and engineering approach
- Case studies are generalized from real-world production experience

---

# 📬 Contact

LinkedIn: https://www.linkedin.com/in/dennis-lubins-25372a356

---

# 📄 License

This repository is licensed under the MIT License.
