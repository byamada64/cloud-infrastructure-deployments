# ☁️ Cloud Infrastructure Deployments | AWS, Azure & Automation

Hands-on cloud infrastructure builds, automation workflows, architecture references, and deployment guides across AWS, Azure, and CI/CD platforms.

## 📌 Lab Scope Note

These guides prioritize build concepts and deployment flow over screenshots, since cloud interfaces evolve frequently while core engineering principles and deployment patterns remain consistent.

Structured headings are intentionally used to serve as a follow-along framework, helping readers navigate each phase of a build while adapting steps to their own environment.


## 📖 About This Repo

This repository contains two types of practical cloud engineering content:

- **Deployments** → Step-by-step build notes showing how resources were provisioned, configured, validated, and cleaned up.
- **Services** → Quick-reference guides explaining what cloud services do, why companies use them, and when to choose them.

Designed for engineers, recruiters, and hiring managers who want real-world examples of both execution and platform understanding.


## 🗂️ Repository Layout

- [`aws/`](./aws/) → AWS builds organized by compute, network, security, containers, and app platforms
- [`azure/`](./azure/) → Azure builds organized by compute, network, security, containers, and app platforms
- [`ci-cd/`](./ci-cd/) → Automation content including Jenkins, Ansible, and Terraform
- [`services/`](./services/) → Service explainers organized by AWS, Azure, and CI/CD tooling


## ☁️ AWS Builds

- [EC2 Instance + NGINX Deployment + Validation](./aws/compute/ec2-deployment.md)
- [Elastic Beanstalk Deployment](./aws/app-platform/elastic-beanstalk-deployment.md)
- [Secrets Manager Deployment](./aws/security/secrets-manager.md)


## 🔷 Azure Builds

- [Azure VM + NGINX Deployment + Validation](./azure/compute/vm-deployment.md)
- [Azure App Service Deployment](./azure/app-platform/app-service-deployment.md)
- [Azure Key Vault Deployment](./azure/security/key-vault-deployment.md)
- [Azure Kubernetes Service (AKS) Deployment](./azure/containers/aks-deployment.md)
- [Azure VM + Storage + Private Endpoint Deployment](./azure/network/vm-storage-private-endpoint.md)
  

## 🔁 CI/CD Builds

- [Jenkins Freestyle and Multibranch Guide](./ci-cd/Jenkins/jenkins-freestyle-and-multibranch-guide.md)
- [Ansible NGINX Deployment](./ci-cd/ansible/ansible-nginx-deployment.md)
- [Ansible Linux Baseline Configuration](./ci-cd/ansible/ansible-linux-baseline.md)
- [Ansible Linux Health Audit](./ci-cd/ansible/ansible-health-audit.md)
- [Ansible Daily Linux Operations Check](./ci-cd/ansible/ansible-daily-ops.md)
- [Terraform VM + Storage Deployment](./ci-cd/terraform/terraform-vm-storage-deployment.md)


## 🧩 Service Guides

### AWS
- [Elastic Beanstalk](./services/aws/elastic-beanstalk.md)
- [Secrets Manager](./services/aws/secrets-manager.md)

### Azure
- [Azure App Service](./services/azure/app-service.md)
- [Azure Key Vault](./services/azure/key-vault.md)
- [Azure Kubernetes Service (AKS)](./services/azure/azure-aks-service.md)
- [Azure VM + Storage + Private Endpoint](./services/azure/azure-vm-storage-container-private-endpoint.md)

### CI/CD
- [Jenkins](./services/ci-cd/jenkins-freestyle-and-multibranch-guide.md)
- [Terraform VM + Storage Deployment](./services/ci-cd/terraform-vm-storage-deployment.md)
- [Ansible NGINX Deployment](./services/ci-cd/ansible-nginx-deployment.md)


## 🛠️ Skills Demonstrated

- AWS & Azure Cloud Platforms
- Infrastructure Provisioning
- Linux Administration
- Web Hosting Deployments
- IAM / Secrets Management
- Networking & Security Concepts
- CI/CD Automation
- Jenkins Pipelines
- Ansible Automation
- Git Workflows
- Monitoring Validation
- Operational Health Checks
- Documentation & Runbooks
- Troubleshooting


## 🎯 Current Focus

- Azure Kubernetes Service (AKS)
- Terraform
- Infrastructure as Code
- Cloud Monitoring
- Multi-cloud Operations
- CI/CD Automation
- Platform Engineering
