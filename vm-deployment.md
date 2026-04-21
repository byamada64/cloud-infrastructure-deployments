# ☁️ Azure VM Provisioning + NGINX Deployment + Validation

## 🎯 Objective

Provisioned a repeatable Azure virtual machine in minutes to validate hosting, security controls, VM health, lifecycle cleanup, and cloud cost governance.

## 🏗️ Environment Build Choices

### 🏷️ Name and Tags

- VM Name: `az-lab-vm`
- Use consistent naming convention for tracking, automation, and scale.

### 💿 Operating System / Image

- Ubuntu Server 24.04 LTS
- Gen2 image
- Standard SSD supported
- 64-bit architecture

### ⚙️ Compute

- VM Size: `B1s`
- Low-cost burstable compute for lab testing

### 🔑 Access Method

- Created new SSH key pair
- Username: `azureuser`
- Downloaded private `.pem` key
- Stored securely after download

### 🌐 Networking

- Virtual Network: New
- Subnet: Default
- Public IP enabled for remote access
- Suitable for lab use; production may use private subnet model

### 🔥 Firewall

- Created dedicated Network Security Group
- SSH (22) allowed
- HTTP (80) allowed
- Rules can later be restricted to source IP

### 💾 Storage

- Default 30GB OS disk
- Changed Premium SSD to Standard SSD
- Delete disk with VM enabled

### 🧩 Advanced Options

- Left default unless workload required customization
- Can later expand for identity, monitoring, scripts, shutdown automation

## 🚀 Deployment Steps

- Launched Azure Ubuntu virtual machine
- Validated VM state = Running
- Captured Public IP address
- Connected via SSH key pair
- Updated packages
- Installed NGINX
- Enabled NGINX service
- Started NGINX service
- Tested browser access via Public IP
- Validated browser and CLI responses

## 💻 Commands Used

- `chmod 400 az-lab-vm_key.pem` — Secure private key permissions
- `ssh -i az-lab-vm_key.pem azureuser@public-ip` — Connect to Azure VM using SSH key
- `sudo apt update -y` — Refresh Ubuntu package repositories
- `sudo apt install nginx -y` — Install NGINX web server
- `sudo systemctl enable nginx` — Enable NGINX to start on boot
- `sudo systemctl start nginx` — Start NGINX service immediately
- `curl -I http://public-ip` — Validate HTTP response headers
- `top` — View real-time CPU and memory utilization

## ✅ Validation

- SSH login successful
- NGINX service active
- HTTP response returned
- Browser page reachable
- VM metrics visible in Azure portal

## 🧠 Lessons Learned

- NSG rules control inbound traffic
- Public IP required for direct internet access
- Small VM sizes are sufficient for lightweight hosting
- Standard SSD is ideal for low-cost labs
- Consistent naming improves management clarity
- Temporary access rules should be removed after testing

## 🔮 Future Improvements

- Terraform Azure VM deployment
- Ansible NGINX install automation
- Azure Monitor alerts
- HTTPS with Let’s Encrypt
- Azure Load Balancer front-end
- Private subnet + Bastion host model
- VM Scale Set testing

## 🧹 Final Hygiene Cleanup

### Cleanup Actions

- Stopped virtual machine
- Selected **Delete VM**
- Confirmed deletion
- Opened **Resource Manager / All Resources**
- Attempted full Resource Group deletion
- Deleted leftover resources manually when needed

### Removed Assets

- Public IP
- Network Security Group
- Virtual Network
- Network Interface
- SSH Key Resource

### Final Validation

- Confirmed no remaining billable assets
- Prevented future charges
- Returned environment to a clean state
