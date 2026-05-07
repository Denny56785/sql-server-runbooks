# 🚀 Phase 1 – Control Plane Setup

---

## 📌 Project Overview

This project is designed to demonstrate end-to-end infrastructure automation and deployment of a containerized SQL Server environment using modern infrastructure and operations tooling.

The goal is to build a repeatable, production-style workflow that provisions infrastructure, configures systems, and deploys workloads using:

* Infrastructure as Code (Terraform)
* Configuration Management (Ansible)
* Containerization (Docker)
* Cloud Infrastructure (AWS EC2)
* SQL Server (containerized workload)

---

## 🎯 Objectives

* Establish a reusable infrastructure deployment pipeline
* Automate system configuration and application deployment
* Integrate SQL Server into a modern Linux-based environment
* Demonstrate multi-layer architecture:

  * Control Plane
  * Infrastructure Layer
  * Workload Layer
* Produce documentation aligned with real-world operational practices

---

## 🧱 Architecture (High-Level)

```
Control Plane (Local)
    ↓
Terraform (Infrastructure Provisioning)
    ↓
AWS EC2 (Linux Instance)
    ↓
Ansible (Configuration Management)
    ↓
Docker (Container Runtime)
    ↓
SQL Server (Containerized Workload)
```

---

## 🧠 Design Approach

This project is structured in phases to ensure:

* Incremental validation of each layer
* Reduced troubleshooting complexity
* Clear separation of responsibilities
* Repeatable and maintainable workflows

Each phase builds on the previous one:

1. Control Plane Setup
2. Infrastructure Provisioning (Terraform)
3. System Configuration (Ansible)
4. Workload Deployment (Docker + SQL Server)
5. Observability and Enhancements

---

# 🚀 Phase 1 – Control Plane Setup

## 📌 Purpose

Establish a dedicated control environment for executing infrastructure and automation workflows.

This environment acts as the **control node**, responsible for:

* Running Terraform
* Executing Ansible playbooks
* Managing AWS resources
* Handling SSH-based automation

---

## 🖥️ Control Plane Environment

### Host System

* Bazzite (immutable Linux distribution)

### Control Environment

* Distrobox container (Fedora-based)

### Distrobox Name

```
iac-control
```

---

## ⚙️ Tools Installed

The following tools were installed within the control environment:

| Tool      | Purpose                            |
| --------- | ---------------------------------- |
| Terraform | Infrastructure provisioning        |
| Ansible   | Configuration management           |
| AWS CLI   | AWS interaction and authentication |
| Git       | Version control                    |
| OpenSSH   | Remote access / automation         |
| Python    | Ansible dependencies               |

---

## 🔧 Setup Steps

### 1. Create Distrobox Environment

```bash
distrobox-create --name iac-control --image fedora:latest
```

### 2. Enter Environment

```bash
distrobox-enter iac-control
```

### 3. Install Base Packages

```bash
sudo dnf update -y
sudo dnf install -y \
  ansible \
  git \
  awscli \
  openssh-clients \
  python3-pip \
  dnf-plugins-core
```

### 4. Add HashiCorp Repository (Required for Terraform)
```bash
sudo dnf config-manager addrepo \
  --from-repofile=https://rpm.releases.hashicorp.com/fedora/hashicorp.repo
```

### 5. Install Terraform

```bash
sudo dnf install -y terraform
```

### 6. Configure AWS CLI

```bash
aws configure
```

Configured with:

* Access Key ID
* Secret Access Key
* Default Region
* Output Format (json)

---

## 🔐 Authentication Setup

### IAM User

* Dedicated IAM user created for automation
* Administrative access initially granted for setup simplicity
* Future phases will refine permissions (least privilege model)

### AWS Budget Controls

* Monthly budget configured
* Threshold alerts:

  * 50%
  * 80%
  * 100%

---

## 🔑 SSH Key Generation

Used for future EC2 access and Ansible automation:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/aws-lab -N ""
```

---

## ✅ Validation

The following commands were used to verify the environment:

```bash
terraform version
ansible --version
aws sts get-caller-identity
```

Successful execution confirms:

* Terraform installed and functional
* Ansible operational
* AWS authentication working

---

## 🧠 Key Outcomes

* Established a dedicated control plane environment
* Verified toolchain functionality
* Configured AWS authentication and access
* Prepared SSH-based connectivity for future automation
* Implemented initial cost-control safeguards

---

## 📍 Next Phase

**Phase 2 – Infrastructure Provisioning (Terraform)**

* Define EC2 instance configuration
* Configure networking and security groups
* Inject SSH key for access
* Validate infrastructure deployment via Terraform

---

## 🧾 Notes

* Terraform installation required adding the HashiCorp repository (not available in default Fedora repos)
* Distrobox environment provides a clean, isolated control node without modifying the host OS
* AWS budget configuration was implemented prior to infrastructure deployment to prevent unexpected costs

---

## 📌 Summary

Phase 1 successfully establishes the foundation for all subsequent automation workflows by creating a fully functional control plane capable of managing infrastructure and system configuration in a repeatable and controlled manner.

---
