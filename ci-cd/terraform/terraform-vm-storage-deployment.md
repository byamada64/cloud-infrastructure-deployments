# Terraform Azure VM + Storage + Networking + Validation

## Build Summary

Created a Terraform VM and storage deployment workflow to evaluate repeatable infrastructure provisioning, configuration consistency, and cloud resource lifecycle control in a production-aligned scenario.

This build focuses on Infrastructure as Code tradeoffs versus manual portal deployment, incorporating Terraform configuration files, provider setup, resource definitions, state awareness, VM provisioning, storage deployment, and validation within a governed lab environment.
## 🏗️ Environment Build Choices

### 🏷️ Naming Convention

* Resource naming used consistent lab standards for tracking, organization, and repeatable deployments.

Examples:

* Resource Group: existing KodeKloud sandbox resource group
* Storage Account: `tfstoragebrian01`
* Blob Container: `tfcontainer`
* Virtual Network: `vm-vnet`
* Subnet: `internal`
* Network Interface: `vm-nic`
* Public IP: `vm-public-ip`
* Virtual Machine: `tfvm01`

* Consistent naming improves automation readability and operational support.

### 💿 Operating System / Image

* Ubuntu Server LTS marketplace image
* Linux-based workload
* Terraform-managed image deployment
* Suitable for lightweight cloud testing and web workloads

### ⚙️ Compute

* Virtual machine size tested: `Standard_B1s`
* Low-cost burstable compute selected for lab provisioning
* Alternative portal-tested sizes reviewed during troubleshooting

### 🔑 Access Method

* Local admin username configured
* Password authentication tested
* SSH key-based access reviewed as preferred production method

### 🌐 Networking

* Terraform deployed full supporting network stack:
  * Virtual Network
  * Subnet
  * Network Interface
  * Public IP Address

* Designed to validate end-to-end infrastructure provisioning.

### 🔥 Security

* Azure networking model used for resource isolation and access
* Future production improvements may include:

  * Network Security Groups
  * Restricted source IP rules
  * Private endpoints
  * Bastion host access

### 💾 Storage

* Standard Azure Storage Account deployed
* Private Blob Container deployed
* Used to validate Terraform stateful resource creation and cleanup

### 🧩 Infrastructure as Code Components

* `providers.tf`
* `main.tf`
* `variables.tf`
* `outputs.tf`

* Used to separate provider config, resources, variables, and outputs.

## 🚀 Deployment Steps

1. Initialized Terraform working directory
2. Installed Azure provider plugins
3. Validated Terraform syntax
4. Generated execution plans before deployment
5. Provisioned Azure Storage Account
6. Provisioned Blob Container
7. Provisioned Virtual Network
8. Provisioned Subnet
9. Provisioned Public IP
10. Provisioned Network Interface
11. Attempted Linux VM deployment
12. Investigated provisioning timeout behavior
13. Validated resources in Azure Portal
14. Removed orphaned resources
15. Destroyed all Terraform-managed resources
16. Confirmed clean environment after teardown

## 💻 Commands Used

* `terraform init` — Initialize Terraform working directory and download providers
* `terraform fmt` — Format Terraform files into standard readable structure
* `terraform validate` — Validate Terraform syntax and configuration logic
* `terraform plan` — Preview infrastructure changes before deployment
* `terraform apply` — Deploy Terraform-managed Azure resources
* `terraform state list` — Display Terraform-managed resources in state file
* `terraform output` — Display configured output values after deployment
* `terraform destroy` — Destroy Terraform-managed infrastructure resources
* `az vm delete --name tfvm01 --resource-group <resource-group> --yes` — Remove orphaned VM after provisioning timeout

## ✅ Validation

* Terraform initialized successfully
* Azure provider authenticated successfully
* Storage Account deployed successfully
* Blob Container deployed successfully
* Virtual Network deployed successfully
* Subnet deployed successfully
* Public IP deployed successfully
* Network Interface deployed successfully
* Resources confirmed in Azure Portal
* Terraform destroy removed remaining managed resources
* Final portal review confirmed clean lab environment

## 🧠 Lessons Learned

* Terraform `plan` previews infrastructure intent before deployment
* Terraform `apply` reveals real cloud policy or provisioning issues
* Valid code can still fail due to platform restrictions
* Terraform state management is critical during partial deployments
* Manual cleanup may be required for orphaned resources
* Portal deployments help validate known-good configurations
* Infrastructure as Code improves repeatability and deployment speed
* Cleanup discipline prevents unnecessary cloud charges

## 🔮 Future Improvements

* Add reusable Terraform modules
* Use SSH key authentication only
* Add Network Security Groups
* Add Azure Bastion access
* Automate NGINX install with cloud-init
* Add Azure Monitor alerts
* Use remote Terraform state storage
* Integrate GitHub Actions CI/CD pipeline
* Separate dev / test / prod environments
* Add Private Endpoint architecture

## 🧹 Final Hygiene Cleanup

* Removed orphaned Linux VM
* Destroyed Network Interface
* Destroyed Public IP
* Destroyed Subnet
* Destroyed Virtual Network
* Destroyed Storage Account
* Destroyed Blob Container
* Verified Terraform state clean
* Verified Azure Portal had no remaining resources
* Practiced responsible cloud governance and lifecycle cleanup
