# EC2 Instance Provisioning + NGINX Deployment + Validation

## 🎯 Objective

Provisioned a repeatable EC2 instance in minutes from a chosen AMI to validate hosting, security controls, instance health, lifecycle cleanup, and cloud cost governance.

## 🏗️ Environment Build Choices

### 🏷️ Name and Tags

* Instance name: `aws-lab-vm`
* Use consistent naming convention for tracking, automation, and scale.

### 💿 Operating System / AMI

* Ubuntu Server 24.04 LTS
* HVM virtualization
* EBS General Purpose SSD
* Free Tier eligible
* 64-bit architecture

### ⚙️ Compute

* Instance type: `t3.micro`
* Low-cost burstable instance for lab testing

### 🔑 Access Method

* Created new SSH key pair
* Key pair name: `aws-lab-pem`
* Type: RSA
* Format: `.pem` for OpenSSH
* Stored securely after download

### 🌐 Networking

* VPC: Default
* Subnet: Selected available AZ subnet
* Public IPv4: Enabled for remote access
* Suitable for lab use; production may use private subnet model

### 🔥 Firewall

* Created dedicated Security Group: `aws-lab-web-sg`
* Avoided default shared Security Group for cleaner access control
* SSH (22) allowed from **My IP**
* HTTP (80) allowed from **My IP** for controlled testing
* Public HTTP access can be changed to `0.0.0.0/0` if external validation is required

### 💾 Storage

* Default root EBS volume
* Free Tier eligible up to 30GB
* Suitable for lightweight Linux web server lab workloads

### 🧩 Advanced Options

* Left default unless workload required customization
* Can later expand for IAM roles, user data scripts, monitoring, or shutdown protection

## 🚀 Deployment Steps

1. Launched EC2 Ubuntu instance
2. Validated instance state = Running
3. Captured Public IPv4 address
4. Connected via SSH key pair
5. Updated packages
6. Installed NGINX
7. Enabled NGINX service
8. Started NGINX service
9. Tested external access via browser using Public IPv4
10. Validated browser and CLI responses

## 💻 Commands Used

* `ssh -i aws-lab-pem.pem ubuntu@public-ip` — Connect to EC2 instance using SSH key
* `sudo apt update -y` — Refresh Ubuntu package repositories
* `sudo apt install nginx -y` — Install NGINX web server
* `sudo systemctl enable nginx` — Enable NGINX to start on boot
* `sudo systemctl start nginx` — Start NGINX service immediately
* `curl -I http://public-ip` — Validate HTTP response headers from web server
* `top` — View real-time CPU and memory utilization

## ✅ Validation

* SSH login successful
* NGINX service active
* HTTP response returned
* Browser page reachable
* Instance metrics visible in EC2 console

## 🧠 Lessons Learned

* Security Groups are stateful firewalls controlling inbound traffic
* Public IPv4 required for direct internet access
* My IP restriction reduces exposure during testing
* Separate Security Groups improve management clarity
* Small instances are sufficient for lightweight web hosting labs

## 🔮 Future Improvements

* Terraform EC2 deployment
* Ansible NGINX install automation
* CloudWatch alarms
* HTTPS with Let’s Encrypt
* Load Balancer front-end
* Private subnet + Bastion host model

## 🧹 Final Hygiene Cleanup

* Terminated EC2 instance after testing completed
* Deleted attached EBS volume to prevent storage charges
* Removed temporary SSH key pair (`aws-lab-pem`)
* Deleted custom Security Group (`aws-lab-web-sg`)
* Verified no remaining billable lab resources in region
* Practiced responsible cloud cost governance
