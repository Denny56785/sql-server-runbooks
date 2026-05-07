# 🚀 Phase 3 – System Configuration with Ansible

---

## 📌 Purpose

Configure the provisioned Ubuntu EC2 instance using Ansible and prepare it as a Docker-ready host for future workload deployment.

This phase establishes the **configuration management layer**, bridging the gap between infrastructure provisioning and containerized application deployment.

---

## 🧱 Architecture (Phase 3 Scope)

```text
Control Plane (Local)
    ↓
Ansible
    ↓
SSH
    ↓
AWS EC2 (Ubuntu Instance)
    ↓
Docker-Ready Host
```

---

## 🧠 Design Approach

This phase uses Ansible to automate system configuration tasks that would otherwise be performed manually over SSH.

The goal is to ensure that the EC2 instance can be rebuilt, configured, and validated consistently from the local control plane.

Key design principles:

* Use SSH-based automation from the local control environment
* Keep Terraform responsible for infrastructure only
* Keep Ansible responsible for operating system configuration
* Prepare the server for Docker Compose workloads
* Validate Docker access without requiring manual sudo usage

---

## 📁 Project Structure

```text
iac-control/
├── terraform/
│   └── ec2-lab/
└── ansible/
    ├── inventory.ini
    ├── ansible.cfg
    └── playbooks/
        └── bootstrap-docker.yml
```

---

## 🔑 SSH Connectivity

Ansible connects to the EC2 instance over SSH using the key created during the control plane setup phase.

```text
Private key: ~/.ssh/aws-lab
Remote user: ubuntu
```

SSH access was validated using:

```bash
ssh -i ~/.ssh/aws-lab ubuntu@<EC2_PUBLIC_IP>
```

---

## 📋 Ansible Inventory

The Ansible inventory defines the EC2 target host and connection details.

```ini
[sql_lab]
sql-lab-01 ansible_host=<EC2_PUBLIC_IP>

[sql_lab:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/aws-lab
ansible_python_interpreter=/usr/bin/python3
```

### Inventory Notes

* `sql-lab-01` provides a readable host alias
* `ansible_host` points to the current EC2 public IP
* `ansible_user` uses the default Ubuntu cloud user
* `ansible_ssh_private_key_file` references the local SSH private key
* `ansible_python_interpreter` ensures Ansible uses Python 3 on the remote host

---

## ⚙️ Ansible Configuration

Ansible project defaults were defined in `ansible.cfg`.

```ini
[defaults]
inventory = inventory.ini
host_key_checking = False
retry_files_enabled = False
stdout_callback = yaml

[ssh_connection]
pipelining = True
```

### Configuration Purpose

| Setting | Purpose |
| ------- | ------- |
| `inventory` | Uses the local inventory file by default |
| `host_key_checking` | Avoids first-run SSH prompt interruptions |
| `retry_files_enabled` | Prevents creation of retry files |
| `stdout_callback` | Improves playbook output readability |
| `pipelining` | Improves SSH execution efficiency |

---

## ✅ Connectivity Validation

Before running system configuration tasks, Ansible connectivity was tested with the ping module:

```bash
cd ~/iac-control/ansible
ansible sql_lab -m ping
```

Expected result:

```text
sql-lab-01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

This confirms:

* SSH connectivity is working
* Ansible can authenticate with the EC2 instance
* Python is available on the remote system
* The inventory configuration is valid

---

## 🐳 Docker Bootstrap Playbook

The Docker bootstrap playbook prepares the Ubuntu EC2 instance for containerized workloads.

```yaml
---
- name: Bootstrap Docker host
  hosts: sql_lab
  become: true

  tasks:
    - name: Update apt package cache
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install required base packages
      ansible.builtin.apt:
        name:
          - ca-certificates
          - curl
          - gnupg
          - lsb-release
        state: present

    - name: Install Docker packages
      ansible.builtin.apt:
        name:
          - docker.io
          - docker-compose-v2
        state: present

    - name: Ensure Docker service is enabled and running
      ansible.builtin.service:
        name: docker
        enabled: true
        state: started

    - name: Add ubuntu user to docker group
      ansible.builtin.user:
        name: ubuntu
        groups: docker
        append: true

    - name: Verify Docker installation
      ansible.builtin.command: docker --version
      register: docker_version
      changed_when: false

    - name: Display Docker version
      ansible.builtin.debug:
        var: docker_version.stdout

    - name: Verify Docker Compose installation
      ansible.builtin.command: docker compose version
      register: docker_compose_version
      changed_when: false

    - name: Display Docker Compose version
      ansible.builtin.debug:
        var: docker_compose_version.stdout
```

---

## ▶️ Playbook Execution

The playbook was executed from the Ansible project directory:

```bash
cd ~/iac-control/ansible
ansible-playbook playbooks/bootstrap-docker.yml
```

The playbook performs the following actions:

* Updates the apt package cache
* Installs base dependency packages
* Installs Docker
* Installs Docker Compose v2
* Enables and starts the Docker service
* Adds the Ubuntu user to the Docker group
* Validates Docker installation
* Validates Docker Compose installation

---

## 🧪 Docker Validation

After the playbook completed, Docker access was validated from the control plane:

```bash
ssh -i ~/.ssh/aws-lab ubuntu@<EC2_PUBLIC_IP> "docker ps"
```

Successful output:

```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

This confirms:

* Docker is installed
* Docker service is running
* The Ubuntu user can run Docker commands without sudo
* The instance is ready for container deployment

---

## ⚠️ Troubleshooting Note

During testing, Ansible initially failed with a connection timeout because the EC2 public IP had changed after the instance was rebuilt.

Resolution:

* Checked the current Terraform output
* Updated `inventory.ini` with the current EC2 public IP
* Retested SSH connectivity
* Re-ran the Ansible connection test

Relevant command:

```bash
cd ~/iac-control/terraform/ec2-lab
terraform output public_ip
```

This highlights an important operational consideration:

```text
When EC2 instances are recreated, public IP addresses may change unless an Elastic IP or dynamic inventory approach is used.
```

---

## 🧠 Key Outcomes

* Established Ansible-based configuration management
* Confirmed SSH automation from the local control plane
* Installed and validated Docker on the Ubuntu EC2 instance
* Installed and validated Docker Compose v2
* Prepared the instance for containerized SQL Server and monitoring workloads
* Reinforced separation of responsibilities between Terraform and Ansible

---

## 📍 Next Phase

**Phase 4 – Containerized Workload Deployment**

Planned components:

* SQL Server Developer Edition container
* AdventureWorks sample database
* Prometheus
* Grafana
* node_exporter
* Docker Compose orchestration

---

## 🧾 Notes

* Terraform remains responsible for provisioning AWS infrastructure
* Ansible is responsible for configuring the operating system
* Docker Compose will be introduced in the next phase to define and run container workloads
* Public IP changes require inventory updates unless automated through dynamic inventory or Terraform output integration

---

## 📌 Summary

Phase 3 successfully configures the Ubuntu EC2 instance as a Docker-ready host using Ansible. This establishes a repeatable configuration management workflow and prepares the environment for deploying SQL Server, monitoring services, and supporting containerized workloads in the next phase.

---
