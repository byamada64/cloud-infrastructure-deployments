# ☁️ Azure VM + NGINX Deployment + Validation + Cleanup

## 🎯 Objective

Provisioned a repeatable Azure Ubuntu virtual machine in minutes to validate lightweight Linux hosting, security controls, web access, lifecycle cleanup, and cloud cost governance.

---

# 🏗️ Environment Build Choices

## 🖥️ Virtual Machine

- Selected **Create > Virtual Machine**

- Chose Ubuntu Server image

- Standard VM size for lab testing

- Used public IP for temporary internet validation

---

# ⚙️ Build Configuration

## 📘 Basics

### 📁 Project Details

- Selected active subscription

- Created / selected Resource Group

### 💻 Instance Details

- VM Name: `az-lab-vm`

- Region: East US

- Availability Zone: Default

- Image: Ubuntu Server 24.04 LTS

### 👤 Administrator Account

- Username: `azureuser`

- Authentication Type: SSH Public Key

- Generated new key pair

### 🌐 Inbound Port Rules

- Allow selected ports:

  - SSH (22)

  - HTTP (80)

> Public access used for lab testing only. Production environments may require private access or approval.

---

## 💽 Disks

- OS Disk: Default 30 GiB

- Changed disk type from Premium to **Standard SSD**

- Delete disk with VM enabled

- Left advanced disk options as default

> Standard SSD chosen for lower-cost lab usage.

---

## 🌐 Networking

- Created new Virtual Network

- Created default subnet

- Created Public IP

- NSG Mode: Basic

- Allowed inbound:

  - SSH (22)

  - HTTP (80)

- Load Balancing: None

> Azure Load Balancer / Application Gateway saved for separate labs.

---

## 🛠️ Management

- Left defaults enabled

- No custom identity or auto-shutdown changes

> Use defaults unless special operational requirements exist.

---

## 📈 Monitoring

- Left default monitoring options

- Boot diagnostics remained default

> Additional monitoring can be enabled in future observability labs.

---

## 🧪 Advanced

- Left extensions / cloud-init defaults

- No custom scripts added during build

> Automation scripts can be added in future IaC / bootstrap labs.

---

## 🏷️ Tags

- Optional for lab

- Useful examples:

| Name | Value |

|------|-------|

| Environment | Lab |

| Owner | Joe |

| Purpose | Training |

> Tags help inventory, reporting, and billing visibility.

---

## ✅ Review + Create

- Selected **Review + Create**

- Validation Passed

- Reviewed final configuration

- Selected **Create**

---

## 🔐 SSH Key Download

Popup appeared after create:

- Selected **Download private key and create resource**

- Saved `.pem` file locally

- Stored securely for SSH access

> Azure does not store the private key again.

---

# 🚀 Deployment Steps

1. Created Azure Ubuntu virtual machine

2. Downloaded SSH private key

3. Waited for deployment success

4. Captured Public IP address

5. Connected through SSH

6. Updated packages

7. Installed NGINX

8. Enabled NGINX service

9. Started NGINX service

10. Tested browser + CLI access

---

# 💻 Commands Used

- `chmod 400 az-lab-vm_key.pem`

- `ssh -i az-lab-vm_key.pem azureuser@public-ip`

- `sudo apt update -y`

- `sudo apt install nginx -y`

- `sudo systemctl enable nginx`

- `sudo systemctl start nginx`

- `curl -I http://public-ip`

- `top`

---

# ✅ Validation

- SSH login successful

- NGINX service active

- HTTP response returned

- Browser page reachable

- VM status visible in Azure portal

---

# 🧠 Lessons Learned

- NSG rules control inbound traffic

- Public IP required for direct testing

- Standard SSD is fine for labs

- Small VM sizes work for lightweight hosting

- Tags improve resource visibility

- Temporary access rules should be removed after use

---

# 🧹 Final Hygiene Cleanup

1. Stopped virtual machine

2. Selected Delete VM

3. Confirmed deletion

4. Opened Resource Manager / All Resources

5. Attempted Resource Group deletion

6. Deleted leftover resources manually

### Removed Assets

- Public IP

- Network Security Group

- Virtual Network

- Network Interface

- SSH Key Resource

### Final Validation

- Confirmed no remaining billable assets

- Prevented future charges

- Environment returned clean
