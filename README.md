# AWS Infrastructure Automation with Terraform & Jenkins

This project demonstrates a complete DevOps workflow for provisioning AWS infrastructure using **Terraform** and automating the deployment process with a **Jenkins CI/CD pipeline**.

## 🚀 Project Overview

This infrastructure-as-code (IaC) project provisions the following AWS components:

- **VPC** with DNS support and two subnets across different Availability Zones.
- **Internet Gateway** to enable public access.
- **Security Group** allowing SSH, HTTP, HTTPS, and ICMP traffic.
- **Four EC2 Instances** (two general-purpose and two tagged for web applications).
- **Automated CI/CD Pipeline** with Jenkins to handle Terraform operations: `init`, `fmt`, `validate`, `plan`, `apply`, and `destroy`.

---

## 🛠️ Tools & Technologies Used

- **Terraform**
- **AWS** (VPC, Subnet, EC2, Security Groups, Internet Gateway)
- **Jenkins**
- **Git**
- **Linux CLI**
- **Infrastructure as Code (IaC)** principles

---

## 📁 Infrastructure Details

### VPC & Networking
- CIDR Block: `10.0.0.0/16`
- Two public subnets in `us-east-1a` and `us-east-1b`
- Internet Gateway attached to the VPC
- Security Group with open rules for:
  - SSH (22)
  - HTTP (80)
  - HTTPS (443)
  - ICMP (All types)

### Compute Instances
- EC2 AMI: `ami-036c2987dfef867fb` (general)
- EC2 AMI: `ami-04b70fa74e45c3917` (web apps)
- Instance Type: `t2.micro`
- Total EC2 Instances: 4

---

## 🔄 Jenkins Pipeline

A Jenkins pipeline automates all stages of Terraform infrastructure management:

```groovy
pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    stages {
        stage('Terraform Init') {
            steps { sh 'terraform init' }
        }
        stage('Terraform Format') {
            steps { sh 'terraform fmt' }
        }
        stage('Terraform Validate') {
            steps { sh 'terraform validate' }
        }
        stage('Terraform Plan') {
            steps { sh 'terraform plan' }
        }
        stage('Terraform Apply') {
            steps { sh 'terraform apply -auto-approve' }
        }
        stage('Terraform Destroy') {
            steps { sh 'terraform destroy -auto-approve' }
        }
    }
}
