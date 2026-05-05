# ⚖️ Azure Load Balancer (VM-Based) Deployment

Hands-on Azure infrastructure build focused on deploying multiple Linux web servers and distributing traffic using Azure Load Balancer with health probes, backend pool management, and failure simulation.


## 🎯 Objective

Design and deploy a highly available web tier using:

- Two Linux Virtual Machines
- NGINX web servers
- Azure Load Balancer (Layer 4)
- Health probes for traffic control

This build demonstrates real-world infrastructure flow including compute provisioning, service validation, load balancing, failure handling, and backend pool modification aligned to migration and operational scenarios.


## 🏗️ Environment Build Choices

### Azure Services Used

- Virtual Machines (Linux)
- Virtual Network (VNet)
- Network Security Groups (NSGs)
- Azure Load Balancer (Standard)
- Public IP Address
- Health Probes


### Architecture Approach

- Build and validate **VMs first (baseline validation)**
- Use **NGINX for lightweight HTTP testing**
- Introduce **Load Balancer after confirming app works**
- Validate:
  - Traffic distribution
  - Health probe behavior
  - Failure scenarios
- Simulate **backend removal (migration use case)**


### Design Decisions

- Standard Load Balancer (production-aligned)
- TCP-based rule (Layer 4)
- HTTP health probe for availability
- Same VNet/subnet requirement for backend pool
- Public frontend IP for testing


## 💻 Commands Used

### SSH Access

- `ssh -i web-vm-01_key.pem azureuser@<VM01_PUBLIC_IP>` — Connect to Web Server 01  
- `ssh -i web-vm-02_key.pem azureuser@<VM02_PUBLIC_IP>` — Connect to Web Server 02  

### Fix Key Permissions

- `chmod 400 web-vm-01_key.pem` — Secure private key for VM-01  
- `chmod 400 web-vm-02_key.pem` — Secure private key for VM-02  

### Install NGINX

- `sudo apt update -y` — Refresh package repositories  
- `sudo apt install nginx -y` — Install NGINX web server  
- `sudo systemctl enable nginx` — Enable service on boot  
- `sudo systemctl start nginx` — Start NGINX service  

### Configure Web Pages

- `echo "OLD APP - Web Server 01" | sudo tee /var/www/html/index.html` — Configure VM-01 response  
- `echo "NEW APP - Web Server 02" | sudo tee /var/www/html/index.html` — Configure VM-02 response  

### Validation

- `curl http://localhost` — Validate local NGINX response  
- `curl http://<VM_PUBLIC_IP>` — Validate external VM access  

### Load Balancer Testing

- `for i in {1..20}; do curl http://<LB_PUBLIC_IP>; done` — Test traffic distribution across backend pool  
- `curl --header "Cache-Control: no-cache" http://<LB_PUBLIC_IP>` — Force fresh request (avoid caching behavior)  

### Failure Simulation

- `sudo systemctl stop nginx` — Simulate application-level failure  
- `sudo systemctl start nginx` — Restore service  
- `sudo shutdown now` — Simulate full VM failure  


## 🔁 ACTUAL BUILD FLOW (Portal-Based — CORRECT ORDER)


### 1️⃣ Create Virtual Machines

Create two Linux VMs:

- `web-vm-01`
- `web-vm-02`

Settings:

- Same VNet and subnet
- Public IP enabled
- Inbound ports:
  - SSH (22)
  - HTTP (80)

Download `.pem` key files

⚠️ Both VMs must be in the SAME VNet for backend pool eligibility  


### 2️⃣ Configure Access to VMs (CRITICAL STEP)

On local machine:

- `chmod 400 web-vm-01_key.pem`
- `chmod 400 web-vm-02_key.pem`

SSH into both VMs

⚠️ Without correct permissions, SSH will fail  


### 3️⃣ Install Web Server (NGINX)

On BOTH VMs:

- `sudo apt update -y`
- `sudo apt install nginx -y`
- `sudo systemctl enable nginx`
- `sudo systemctl start nginx`

⚠️ Ensure service is running before moving forward  


### 4️⃣ Configure Unique Web Responses

On VM-01:

- `echo "OLD APP - Web Server 01" | sudo tee /var/www/html/index.html`

On VM-02:

- `echo "NEW APP - Web Server 02" | sudo tee /var/www/html/index.html`

⚠️ Required to visually confirm load balancing behavior  


### 5️⃣ Validate VM Connectivity (MANDATORY BEFORE LB)

From local machine:

- `curl http://<VM01_PUBLIC_IP>`
- `curl http://<VM02_PUBLIC_IP>`

Expected:

- VM-01 returns OLD APP
- VM-02 returns NEW APP

⚠️ If this fails → STOP  
⚠️ Fix NSG / NGINX before building LB  


### 6️⃣ Create Azure Load Balancer

- Name: `web-lb`
- SKU: Standard
- Type: Public
- Tier: Regional

⚠️ Basic SKU not used (non-production behavior)  

### 🌐 Public IP (Pre-Creation Step)

Created Public IP separately before attaching to Load Balancer:

- Name: `web-lb-pip`
- SKU: Standard
- Assignment: Static

Navigation:

- Azure Portal → Public IP addresses → Create

⚠️ This Public IP is attached in the next step (Frontend IP)  
⚠️ Preferred over auto-created IP for naming and control  


### 7️⃣ Configure Frontend IP

- Name: `web-lb-fe`
- Public IP: `web-lb-pip` (existing)

⚠️ Required entry point for external traffic  


### 8️⃣ Create Backend Pool

- Name: `web-backend-pool`

Add:

- web-vm-01
- web-vm-02

⚠️ Only VMs in same VNet will appear  


### 9️⃣ Configure Health Probe

- Name: `http-probe`
- Protocol: HTTP
- Port: 80
- Path: /

⚠️ Health probe determines traffic eligibility  


### 🔟 Create Load Balancing Rule

- Name: `web-http-rule`
- Protocol: TCP
- Port: 80 → 80
- Backend pool: web-backend-pool
- Health probe: http-probe
- Session persistence: None

⚠️ This ties frontend → backend pool  


### 1️⃣1️⃣ Validate Load Balancer

Run:

- `for i in {1..20}; do curl http://<LB_PUBLIC_IP>; done`

Expected:

OLD APP - Web Server 01  
NEW APP - Web Server 02  

⚠️ Traffic is hash-based (not strict round robin)  


### 1️⃣2️⃣ Observe Cache Behavior

- `curl http://<LB_PUBLIC_IP>`

May return same server repeatedly  

Force refresh:

- `curl --header "Cache-Control: no-cache" http://<LB_PUBLIC_IP>`


### 1️⃣3️⃣ Simulate Failure

On VM-01:

- `sudo systemctl stop nginx`

Test again:

- `for i in {1..10}; do curl http://<LB_PUBLIC_IP>; done`

Expected:

NEW APP - Web Server 02  

⚠️ Health probe removes failed instance automatically  


### 1️⃣4️⃣ Restore Service

- `sudo systemctl start nginx`

Traffic resumes across both servers  


### 1️⃣5️⃣ Simulate Backend Removal (Migration Scenario)

Attempt to remove VM-02 directly → FAIL  

⚠️ Azure blocks removal due to rule dependency  


### 1️⃣6️⃣ Correct Removal Process

Step A — Detach Rule  
- Load Balancer → Load balancing rules  
- Edit `web-http-rule`  
- Set Backend Pool = None  
- Save  

Step B — Remove VM  
- Backend Pools → web-backend-pool  
- Remove `web-vm-02`  
- Save  

Step C — Reattach Rule  
- Set Backend Pool = web-backend-pool  
- Save  


### 1️⃣7️⃣ Final Validation

- `for i in {1..10}; do curl http://<LB_PUBLIC_IP>; done`

Expected:

OLD APP - Web Server 01  


## ✅ Validation

### Confirmed

- Both VMs accessible independently  
- Load Balancer distributes traffic  
- Health probe removes unhealthy instance  
- Backend removal works after rule detachment  


## 🧯 Lessons Learned

### Issues Encountered

- SSH failures due to incorrect key permissions  
- Load Balancer initially appeared “sticky” (cache behavior)  
- Backend pool removal blocked by rule dependency  
- Temporary connection timeouts during state changes  


### Fixes Applied

- Applied chmod 400 to keys  
- Forced no-cache curl requests  
- Detached LB rule before pool modification  
- Restarted NGINX to restore health  


### Key Takeaways

- Always validate VMs before introducing LB  
- Health probes control traffic, not VM state alone  
- Build order matters in Azure  
- Backend pools are locked when tied to rules  
- Load balancing is hash-based, not round-robin  
- Observability tools may lag behind real state  


## 🚀 Future Improvements

- Blue/Green deployment using backend pools  
- Azure Application Gateway (Layer 7)  
- HTTPS + SSL termination  
- VM Scale Sets for autoscaling  
- Custom health probe endpoints  


## 🧹 Final Hygiene Cleanup

### Cleanup Performed

None during initial deployment  


### Recommended Cleanup

- Delete Load Balancer  
- Delete Public IP  
- Delete Virtual Machines  
- Remove unused NICs  
- Delete Resource Group (lab only)  
