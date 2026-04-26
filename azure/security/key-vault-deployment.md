# 🔐 Azure Key Vault Deployment

Provisioned and validated an Azure Key Vault to demonstrate secure credential storage, RBAC access control, secret lifecycle management, and enterprise cloud governance.


## 🏗️ Environment Build Choices

### Project Details

- **Subscription:** Existing assigned lab subscription provided by organization  
- **Resource Group:** Existing approved Resource Group provided by lab environment  

### Notes

- User should create a dedicated Resource Group when permissions allow  
- In governed enterprise or sandbox environments, preassigned subscriptions and Resource Groups are common  
- Existing resources were used due to access limitations in the lab tenant  

### Instance Details

- **Vault Name:** kv-lab-eastus  
- **Region:** East US  
- **Pricing Tier:** Standard

### Security Configuration

- **Soft Delete:** Enabled  
- **Retention Period:** 7 Days  
- **Purge Protection:** Disabled

### Access Configuration

- **Permission Model:** Azure role-based access control (RBAC)  
- **Azure VM Deployment Access:** Disabled  
- **ARM Template Deployment Access:** Disabled  
- **Disk Encryption Access:** Disabled

### Networking

- **Public Access:** Enabled  
- **Allowed Networks:** All Networks  
- **Private Endpoint:** Not configured

### Tags

- None assigned during deployment


## 💻 Commands Used

- Azure Portal used for full Key Vault deployment workflow  
- No CLI or PowerShell commands required during initial build  


## ✅ Validation

- Key Vault deployed successfully  
- Vault URI generated  
- Standard tier confirmed  
- Secrets created successfully  
- Secret retrieval available  
- IAM controls visible  
- Monitoring options available  
- ARM template export available  
- Soft delete enabled  
- Resource accessible from portal
- Secret created: vm-admin-password (mock-up)
- Secret created: sql-prod-admin-password (mock-up)
- Secret created: servicenow-api-token (mock-up)

## 🧠 Lessons Learned

- Azure Key Vault securely stores passwords, tokens, certificates, and encryption material  
- RBAC determines who can view or manage secrets  
- Secret versioning supports credential rotation without downtime  
- Soft delete protects against accidental deletion  
- Governance settings may be enforced by subscription policy  
- Enterprise environments commonly restrict creation of subscriptions or Resource Groups  
- Key Vault aligns closely with enterprise PAM solutions such as BeyondTrust and CyberArk  

## 🔮 Future Improvements

- Create dedicated Resource Group when permissions allow  
- Integrate private endpoint access  
- Connect applications using Managed Identity  
- Enable certificate auto-renewal workflows  
- Add alerting for secret expiration  
- Automate deployments with Terraform / Bicep  
- Enable purge protection in production scenarios  

## 🧹 Final Hygiene Cleanup

- Deleted test secrets after validation  
- Deleted Key Vault resource after testing  
- Existing shared Resource Group retained (lab-owned resource)  
- Confirmed no leftover billable resources remained  
