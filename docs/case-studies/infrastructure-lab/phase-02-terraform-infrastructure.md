# 🚀 Phase 2 – Infrastructure Provisioning (Terraform)

---

## 📌 Purpose

Provision cloud infrastructure using Infrastructure as Code (IaC) to create a repeatable, controlled, and secure deployment of a Linux-based compute environment in AWS.

This phase establishes the **infrastructure layer**, which will later be configured and utilized by Ansible and Docker in subsequent phases.

---

## 🧱 Architecture (Phase 2 Scope)

```text
Control Plane (Local)
    ↓
Terraform
    ↓
AWS EC2 (Ubuntu Instance)
```

---

## 🧠 Design Approach

This phase focuses on simplicity, reproducibility, and security:

* Use Terraform from the local control plane to provision AWS resources
* Leverage AWS default VPC to reduce initial complexity
* Dynamically select a supported Ubuntu LTS AMI
* Restrict SSH access to a single trusted public IP
* Manage SSH key pairs via Terraform for full IaC consistency
* Output connection details for downstream automation (Ansible)

---

## 📁 Project Structure

```text
iac-control/
└── terraform/
    └── ec2-lab/
        ├── providers.tf
        ├── variables.tf
        ├── main.tf
        ├── outputs.tf
        ├── terraform.tfvars.example
        ├── terraform.tfvars   # local only (ignored)
        ├── .gitignore
        └── README.md
```

---

## ⚙️ Terraform Configuration

### Providers

Defines Terraform version and AWS provider:

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}
```

---

### Variables

Defines configurable inputs for deployment:

```hcl
variable "aws_region" {
  description = "AWS region where resources will be created."
  type        = string
  default     = "us-east-2"
}

variable "project_name" {
  description = "Project name used for tagging resources."
  type        = string
  default     = "terraform-ec2-lab"
}

variable "environment" {
  description = "Environment name."
  type        = string
  default     = "lab"
}

variable "instance_type" {
  description = "EC2 instance type."
  type        = string
  default     = "t3.micro"
}

variable "ssh_key_name" {
  description = "Name of the AWS key pair."
  type        = string
}

variable "ssh_allowed_cidr" {
  description = "CIDR block allowed for SSH access."
  type        = string
}

variable "public_key_path" {
  description = "Path to local SSH public key."
  type        = string
  default     = "~/.ssh/aws-lab.pub"
}
```

---

### Example Variable File

```hcl
aws_region        = "us-east-2"
project_name      = "terraform-ec2-lab"
environment       = "lab"
instance_type     = "t3.small"
ssh_key_name      = "iac-control-key"
ssh_allowed_cidr  = "X.X.X.X/32"
public_key_path   = "~/.ssh/aws-lab.pub"
```

---

### Core Infrastructure (main.tf)

Key components:

* Default VPC lookup
* Subnet discovery
* Ubuntu AMI selection (dynamic)
* SSH key pair creation from local public key
* Security group with restricted SSH access
* EC2 instance deployment

```hcl
data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  filter {
    name   = "state"
    values = ["available"]
  }
}

resource "aws_key_pair" "lab_key" {
  key_name   = var.ssh_key_name
  public_key = file(pathexpand(var.public_key_path))
}

resource "aws_security_group" "lab_sg" {
  name        = "${var.project_name}-sg"
  description = "Security group for Terraform EC2 lab"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "SSH access"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = [var.ssh_allowed_cidr]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "lab_server" {
  ami                         = data.aws_ami.ubuntu.id
  instance_type               = var.instance_type
  subnet_id                   = data.aws_subnets.default.ids[0]
  vpc_security_group_ids      = [aws_security_group.lab_sg.id]
  key_name                    = aws_key_pair.lab_key.key_name
  associate_public_ip_address = true

  metadata_options {
    http_tokens = "required"
  }

  root_block_device {
    volume_size = 20
    volume_type = "gp3"
  }

  tags = {
    Name        = "${var.project_name}-ubuntu"
    Project     = var.project_name
    Environment = var.environment
  }
}
```

---

### Outputs

```hcl
output "instance_id" {
  value = aws_instance.lab_server.id
}

output "public_ip" {
  value = aws_instance.lab_server.public_ip
}

output "public_dns" {
  value = aws_instance.lab_server.public_dns
}

output "ssh_command" {
  value = "ssh -i ~/.ssh/aws-lab ubuntu@${aws_instance.lab_server.public_ip}"
}

output "ubuntu_ami_id" {
  value = data.aws_ami.ubuntu.id
}
```

---

## 🔧 Execution Workflow

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Review Plan

```bash
terraform plan
```

### Apply Infrastructure

```bash
terraform apply
```

---

## 🔐 Security Design

* SSH access restricted to a single public IP (`/32`)
* No credentials stored in Terraform files
* SSH keys managed via local system and injected into AWS
* IMDSv2 enforced via metadata options
* Security groups explicitly defined

---

## 🔑 SSH Access

After deployment:

```bash
ssh -i ~/.ssh/aws-lab ubuntu@<PUBLIC_IP>
```

---

## 💰 Cost Management

Recommended workflow:

```bash
terraform apply   # create resources
terraform destroy # remove resources when not in use
```

This ensures:

* No unnecessary compute charges
* Clean, reproducible deployments
* No orphaned infrastructure

---

## ✅ Validation

Successful deployment confirmed by:

* Terraform apply completes without errors
* Public IP and DNS are returned
* SSH connection to EC2 instance is successful

---

## 🧠 Key Outcomes

* Infrastructure successfully provisioned using Terraform
* Dynamic Ubuntu AMI selection implemented
* Secure SSH access configured
* Local-to-cloud deployment pipeline validated
* Infrastructure lifecycle (create/destroy) established

---

## 📍 Next Phase

**Phase 3 – System Configuration (Ansible)**

* Configure EC2 instance via Ansible
* Install Docker and dependencies
* Prepare system for containerized workloads

---

## 🧾 Notes

* Ubuntu AMI selection required updated filter path (`gp3` images)
* Default VPC used to simplify initial deployment
* `.terraform.lock.hcl` retained for provider consistency
* `terraform.tfvars` excluded from version control for security
* Public IP must be updated if network changes before redeployment

---

## 📌 Summary

Phase 2 establishes a fully functional infrastructure layer using Terraform, enabling consistent, secure, and repeatable deployment of cloud resources. This forms the foundation for automated configuration and workload deployment in subsequent phases.

---
