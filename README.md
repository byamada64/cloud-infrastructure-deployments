# ☁️ Cloud Infrastructure & Platform Engineering | AWS, Azure, CI/CD

Hands-on cloud infrastructure and platform engineering builds across AWS, Azure, and CI/CD tooling.

This repository demonstrates real-world provisioning, automation, and operational validation using repeatable deployment patterns rather than UI-driven workflows.

## 📌 Lab Scope

These builds focus on core deployment patterns and operational workflows rather than screenshots, since cloud interfaces change frequently while engineering principles remain consistent.

Structured headings are used as a repeatable framework to guide build, validation, and cleanup phases.


## 📖 About This Repo

This repository is organized into two complementary areas:

- **Deployments** → Hands-on builds showing how infrastructure is provisioned, configured, validated, and cleaned up  
- **Services** → Reference guides explaining service capabilities, tradeoffs, and when to use one solution over another  

The goal is to demonstrate both **execution (hands-on engineering)** and **decision-making (architecture awareness)** across cloud platforms.

Designed for engineers, recruiters, and hiring managers evaluating real-world cloud and automation experience.


## 🗂️ Repository Layout

- **aws/** → AWS builds (compute, networking, security, containers, app platforms)  
- **azure/** → Azure builds (compute, networking, security, containers, app platforms)  
- **ci-cd/** → Automation workflows (Jenkins, Ansible, Terraform)  
- **services/** → Service reference guides (AWS, Azure, CI/CD tools)


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

**Cloud Platforms**  
AWS, Azure  

**Infrastructure & Networking**  
Compute provisioning, virtual machines, storage, private connectivity, network security  

**Security & Access Control**  
IAM, RBAC, secrets management, Key Vault, KMS  

**Automation & CI/CD**  
Jenkins, Ansible, Terraform, Infrastructure as Code  

**Systems & Operations**  
Linux administration, service validation, monitoring, health checks  

**Engineering Practices**  
Troubleshooting, runbooks, repeatable builds, operational validation  


## 🎯 Current Focus

- Azure Kubernetes Service (AKS)
- Terraform & Infrastructure as Code
- Cloud Monitoring & Observability
- Multi-cloud Operations
- CI/CD & Platform Engineering
