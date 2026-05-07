# Building an End-to-End Infrastructure, Deployment & Observability Lab on AWS

## Overview

This case study outlines the design and implementation of a full-stack infrastructure and workload deployment lab using Terraform, Ansible, Docker, and SQL Server.

The project demonstrates how to move from **infrastructure provisioning → workload deployment → monitoring → automated database recovery**, resulting in a fully operational and observable system.

---

## Environment

* AWS EC2 (Ubuntu)
* Terraform (infrastructure provisioning)
* Ansible (configuration management)
* Docker / Docker Compose (workload orchestration)
* SQL Server 2022 (Developer Edition on Linux)
* Prometheus (metrics collection)
* Grafana (visualization)
* Sample databases:

  * AdventureWorks (manual validation)
  * WideWorldImporters (automated recovery)

---

## Problem

Modern infrastructure roles require the ability to:

* Provision systems in the cloud
* Configure environments consistently
* Deploy workloads in a reproducible way
* Monitor systems in real-time
* Automate operational tasks such as database recovery

Most environments handle these responsibilities in isolation, but rarely demonstrate them as a **single cohesive system**.

---

## Objective

Design and implement a lab that demonstrates:

```text
Infrastructure → Configuration → Deployment → Monitoring → Automation
```

With a focus on:

* Repeatability
* Observability
* Operational realism
* Minimal but meaningful architecture

---

## Architecture

```text
AWS EC2 (Ubuntu)
└── Docker / Docker Compose
    ├── SQL Server (Primary Workload)
    ├── Prometheus (Metrics Collection)
    ├── Grafana (Visualization)
    └── node_exporter (Host Metrics)
```

---

## Implementation Phases

### Phase 1 – Control Plane Setup

→ [View Phase 1](./infrastructure-lab/phase-01-control-plane-setup.md)

* Local control environment configured
* Terraform, Ansible, and AWS CLI installed
* Authentication and access configured

---

### Phase 2 – Infrastructure Provisioning (Terraform)

→ [View Phase 2](./infrastructure-lab/phase-02-terraform-infrastructure.md)

* EC2 instance provisioned via Terraform
* Security groups configured (SSH, Grafana, Prometheus)
* Outputs exposed for connectivity

---

### Phase 3 – Configuration Management (Ansible)

→ [View Phase 3](./infrastructure-lab/phase-03-ansible-configuration.md)

* Docker and dependencies installed via Ansible
* Remote host prepared for workload deployment
* Connectivity validated from control plane

---

### Phase 4 – Workload Deployment, Monitoring & Automation

→ [View Phase 4](./infrastructure-lab/phase-04-workload-monitoring-automation.md)

* Docker Compose stack deployed:

  * SQL Server
  * Prometheus
  * Grafana
  * node_exporter
* Monitoring pipeline established (Prometheus → Grafana)
* AdventureWorks restored manually for validation
* WideWorldImporters restored via automated Ansible workflow

---

## Key Challenges

### 1. Resource Constraints

* SQL Server container requires minimum 2GB RAM
* Initial deployment on free-tier instance caused container failures

**Resolution:**

* Upgraded instance type to meet workload requirements

---

### 2. Container Networking & Access

* Services accessible internally but not externally

**Resolution:**

* Configured Terraform security group rules for:

  * Grafana (3000)
  * Prometheus (9090)

---

### 3. Monitoring Pipeline Integration

* Prometheus scraping confirmed, but dashboard alignment required

**Resolution:**

* Validated metrics directly in Prometheus
* Established working observability pipeline independent of dashboard complexity

---

### 4. Database Recovery Workflow

* Manual restore validated functionality but lacked repeatability

**Resolution:**

* Implemented automated recovery via Ansible:

  * File transfer
  * Container copy
  * SQL restore execution

---

## Solution

The final system integrates:

```text
Terraform → Ansible → Docker → SQL Server → Prometheus → Grafana
```

With:

* Manual validation (AdventureWorks)
* Automated provisioning (WideWorldImporters)
* Real-time monitoring (host + workload visibility)

---

## Operationalization

This project demonstrates a repeatable operational workflow:

1. Provision infrastructure
2. Configure environment
3. Deploy workload
4. Validate system behavior
5. Automate recovery processes

---

## Results

* Fully functional cloud-based lab environment
* Successful deployment of multi-container workload
* Real-time monitoring pipeline operational
* Automated database recovery implemented
* End-to-end system validated with real data

### Measurable Outcomes

* Reduced manual effort through automation
* Improved visibility into system performance
* Established repeatable deployment workflow
* Demonstrated integration of multiple operational layers

---

## Key Takeaways

* Infrastructure and operations are most valuable when integrated, not isolated
* Validation should precede automation to ensure correctness
* Monitoring is critical for understanding system behavior post-deployment
* Automation transforms one-time actions into repeatable systems
* Simplicity in design leads to clearer operational outcomes

---

## Next Steps / Enhancements

* Expand SQL Server metric collection (Query Store, wait stats)
* Build custom Grafana dashboards for SQL observability
* Implement alerting strategies (resource thresholds, service health)
* Extend architecture to multi-node or multi-environment deployments

---

## Summary

This project demonstrates the ability to design and implement a complete operational system from the ground up, including:

* Cloud infrastructure provisioning
* Configuration management
* Containerized workload deployment
* Database validation and recovery
* Monitoring and observability

The result is a **production-style deployment and automation model** that reflects real-world infrastructure and operations workflows.

---
