# Production Ready ECS Fargate Infrastructure using Ansible

Production-grade AWS ECS Fargate infrastructure automation using Ansible.

This project creates and manages:

- VPC
- Public & Private Subnets
- Internet Gateway
- Route Tables
- Security Groups
- Application Load Balancer (ALB)
- ECS Cluster
- ECS Services
- ECS Task Definitions
- ECR Repository
- CloudWatch Logs
- ECS AutoScaling

---

# Architecture

```text
Internet
    |
    v
Application Load Balancer
    |
    v
ECS Fargate Service
    |
    v
Docker Containers
```

---

# Features

- ECS Fargate Deployment
- Dynamic ECS Services
- Dynamic Task Scaling
- AutoScaling Support
- ALB Integration
- ECR Integration
- CloudWatch Logging
- Production-ready folder structure
- GitHub Actions CI/CD ready

---

# Project Structure

```bash
ecs-production/
│
├── inventories/
│   └── prod/
│       └── hosts.yml
│
├── group_vars/
│   └── all.yml
│
├── roles/
│   └── ecs-platform/
│       ├── tasks/
│       ├── templates/
│       ├── defaults/
│       └── vars/
│
├── deploy.yml
│
└── .github/
    └── workflows/
```

---

# Prerequisites

Install:

- Python 3
- Ansible
- AWS CLI
- boto3
- botocore

---

# Install Dependencies

## Create Virtual Environment

```bash
python3 -m venv venv
```

Activate:

```bash
source venv/bin/activate
```

Install packages:

```bash
pip install ansible boto3 botocore
```

Install collections:

```bash
ansible-galaxy collection install amazon.aws
ansible-galaxy collection install community.aws
```

---

# AWS Credentials

Configure AWS CLI:

```bash
aws configure
```

Add:

- AWS Access Key
- AWS Secret Key
- Region

Example:

```text
AWS Region: ap-south-1
```

---

# Configuration

Main configuration file:

```bash
group_vars/all.yml
```

---

# Change AWS Region

```yaml
aws_region: ap-south-1
```

---

# Change VPC CIDR

```yaml
vpc_cidr: 10.0.0.0/16
```

---

# Change Public Subnets

```yaml
public_subnets:
  - 10.0.1.0/24
  - 10.0.2.0/24
```

---

# Change Private Subnets

```yaml
private_subnets:
  - 10.0.10.0/24
  - 10.0.20.0/24
```

---

# Change Availability Zones

```yaml
availability_zones:
  - ap-south-1a
  - ap-south-1b
```

---

# Change ECS Cluster Name

```yaml
ecs_cluster_name: prod-ecs
```

---

# Configure ECS Services

```yaml
ecs_services:
  - name: backend
    image: backend:latest
    cpu: 1024
    memory: 2048
    desired_count: 2
    container_port: 8000
    min_capacity: 2
    max_capacity: 5
```

---

# CPU & Memory Values

| CPU | Memory |
|-----|--------|
| 256 | 512 MB |
| 512 | 1 GB |
| 1024 | 2 GB |
| 2048 | 4 GB |

---

# Where to Add AWS Account ID

File:

```bash
roles/ecs-platform/tasks/task-definition.yml
```

Replace:

```yaml
execution_role_arn: "arn:aws:iam::YOUR_ACCOUNT_ID:role/ecsTaskExecutionRole"
```

Example:

```yaml
execution_role_arn: "arn:aws:iam::423685520063:role/ecsTaskExecutionRole"
```

---

# Create ECS Execution Role

Run:

```bash
aws iam create-role \
--role-name ecsTaskExecutionRole \
--assume-role-policy-document file://trust-policy.json
```

Attach policy:

```bash
aws iam attach-role-policy \
--role-name ecsTaskExecutionRole \
--policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy
```

---

# Deploy Infrastructure

Run:

```bash
ansible-playbook -i inventories/prod/hosts.yml deploy.yml
```
---
# Author

Prashant Yadav

Senior DevOps Engineer
