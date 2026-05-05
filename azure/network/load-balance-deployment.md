# ⚖️ Azure Load Balancer (VM-Based) Deployment

Hands-on Azure infrastructure build focused on deploying multiple Linux web servers and distributing traffic using Azure Load Balancer with health probes, backend pool management, failure simulation, and migration-style backend removal.


## 🎯 Objective

Design and deploy a highly available web tier using:

- Two Linux Virtual Machines
- NGINX web servers
- Azure Load Balancer (Layer 4)
- Health probes for traffic control
- Backend pool operations for failure and migration scenarios

This build demonstrates real-world infrastructure flow including compute provisioning, service validation, load balancing, failure handling, recovery testing, and backend pool modification.


## 🏗️ Environment Build Choices

### Azure Services Used

- Virtual Machines (Linux)
- Virtual Network (VNet)
- Network Security Groups (NSGs)
- Azure Load Balancer (Standard)
- Public IP Address
- Health Probes


### Architecture Approach

- Build and validate VMs first before introducing the Load Balancer
- Use NGINX for lightweight HTTP testing
- Use Azure Load Balancer for Layer 4 traffic distribution
- Validate backend health using both curl and Azure Insights
- Simulate backend failure and backend removal scenarios


## Design Decisions

- Standard Load Balancer for production-aligned behavior  
- Public Load Balancer for browser and curl-based testing  
- TCP-based rule using port 80  
- HTTP health probe using `/`  
- Same VNet/subnet for backend pool eligibility  
- Public IP created separately for naming and control  


## 🔁 ACTUAL BUILD FLOW (Portal-Based — CORRECT ORDER)


## 🏗️ VM & Base Infrastructure Build

### 1️⃣ Create Virtual Machines

Create two Linux VMs:

- web-vm-01
- web-vm-02

Settings:

- Same region
- Same VNet and subnet
- Public IP enabled
- Inbound ports:
  - SSH (22)
  - HTTP (80)

Download `.pem` key files for both VMs.

Important:

- Both VMs must be in the SAME VNet for backend pool eligibility.


### 2️⃣ Configure Access & SSH

On local machine:

- `chmod 400 web-vm-01_key.pem` — Sets private key to read-only (owner only, required for SSH)
- `chmod 400 web-vm-02_key.pem` — Sets private key to read-only (owner only, required for SSH)

Connect to both VMs:

- `ssh -i web-vm-01_key.pem azureuser@<VM01_PUBLIC_IP>` — Connect to Web Server 01
- `ssh -i web-vm-02_key.pem azureuser@<VM02_PUBLIC_IP>` — Connect to Web Server 02


### 3️⃣ Install & Configure NGINX

Run on BOTH VMs:

- `sudo apt update -y` — Refresh package repositories
- `sudo apt install nginx -y` — Install NGINX web server
- `sudo systemctl enable nginx` — Enable NGINX to start on boot
- `sudo systemctl start nginx` — Start NGINX service

Important:

- Confirm NGINX is installed and running before building the Load Balancer.


### 4️⃣ Configure Unique Web Responses

On VM-01:

- `echo "OLD APP - Web Server 01" | sudo tee /var/www/html/index.html` — Configure VM-01 test response

On VM-02:

- `echo "NEW APP - Web Server 02" | sudo tee /var/www/html/index.html` — Configure VM-02 test response

Important:

- Unique responses make it easy to confirm which backend is serving traffic.


## 🧪 Baseline Validation Before Load Balancer

### 1️⃣ Validate Local NGINX Response

Run on each VM:

- `curl http://localhost` — Validate local NGINX response

Expected:

- VM-01 returns `OLD APP - Web Server 01`
- VM-02 returns `NEW APP - Web Server 02`


### 2️⃣ Validate Direct VM Access

Run from local machine:

- `curl http://<VM01_PUBLIC_IP>` — Validate external access to VM-01
- `curl http://<VM02_PUBLIC_IP>` — Validate external access to VM-02

Expected:

- VM-01 returns `OLD APP - Web Server 01`
- VM-02 returns `NEW APP - Web Server 02`

Important:

- If direct VM validation fails, fix NSG rules or NGINX before creating the Load Balancer.


## ⚖️ Load Balancer Deployment

### 1️⃣ Create Azure Load Balancer

Create Load Balancer:

- Name: `web-lb`
- SKU: Standard
- Type: Public
- Tier: Regional

Important:

- Basic SKU was not used because Standard is more production-aligned.


### 🌐 Public IP Pre-Creation Step

Create Public IP separately before attaching it to the Load Balancer:

- Name: `web-lb-pip`
- SKU: Standard
- Assignment: Static

Navigation:

- Azure Portal → Public IP addresses → Create

Important:

- This Public IP is attached during frontend configuration.
- Creating it separately improves naming consistency and resource control.


### 2️⃣ Configure Frontend IP

Configure frontend IP:

- Name: `web-lb-fe`
- Public IP: `web-lb-pip` existing Public IP

Purpose:

- This becomes the external entry point for HTTP traffic.


### 3️⃣ Create Backend Pool

Create backend pool:

- Name: `web-backend-pool`

Add backend VMs:

- `web-vm-01`
- `web-vm-02`

Important:

- Only VMs in the same VNet will appear as eligible backend targets.


### 4️⃣ Configure Health Probe

Create health probe:

- Name: `http-probe`
- Protocol: HTTP
- Port: 80
- Path: `/`

Purpose:

- Health probe determines which backend instances are eligible to receive traffic.


### 5️⃣ Create Load Balancing Rule

Create load balancing rule:

- Name: `web-http-rule`
- Protocol: TCP
- Frontend port: 80
- Backend port: 80
- Frontend IP: `web-lb-fe`
- Backend pool: `web-backend-pool`
- Health probe: `http-probe`
- Session persistence: None

Purpose:

- This connects the public frontend IP to the backend VM pool.


### 6️⃣ Skip Outbound Rules

Outbound rules were skipped.

Reason:

- This lab focuses on inbound HTTP traffic from Internet → Load Balancer → backend VMs.
- Outbound SNAT tuning is a separate NAT Gateway / egress control topic.


## 🔄 Traffic Validation & Behavior

### 1️⃣ Validate Load Distribution

Run from local machine:

- `for i in {1..20}; do curl http://<LB_PUBLIC_IP>; done` — Test traffic distribution across backend pool

Expected:

- `OLD APP - Web Server 01`
- `NEW APP - Web Server 02`

Important:

- Azure Load Balancer traffic distribution is hash-based, not strict round-robin.


### 2️⃣ Force Fresh Request

Run from local machine:

- `curl --header "Cache-Control: no-cache" http://<LB_PUBLIC_IP>` — Force fresh request and reduce cached response behavior

Purpose:

- Helps validate backend switching when browser or client behavior appears sticky.


### 3️⃣ Azure Portal Validation

Navigation:

- Azure Portal → Load Balancer → `web-lb`
- Monitoring → Insights

Check:

- `web-vm-01` → Healthy green
- `web-vm-02` → Healthy green

Purpose:

- Confirms both backend VMs are healthy and eligible for traffic.


## 💥 Failure Simulation

### 1️⃣ Simulate Application Failure

Run on VM-01:

- `sudo systemctl stop nginx` — Simulate application-level failure

Purpose:

- Stops the web service while leaving the VM running.


### 2️⃣ Validate Failover Behavior

Run from local machine:

- `for i in {1..10}; do curl http://<LB_PUBLIC_IP>; done` — Confirm traffic routes only to healthy backend

Expected:

- `NEW APP - Web Server 02`

Important:

- Health probe should remove VM-01 from active rotation after NGINX stops.


### 3️⃣ Azure Portal Failure Validation

Navigation:

- Azure Portal → Load Balancer → `web-lb`
- Monitoring → Insights

Expected:

- `web-vm-01` → Unhealthy red
- `web-vm-02` → Healthy green

Important:

- VM-01 can still be running while the application is unhealthy.
- Load Balancer health is based on probe response, not VM power state alone.


## 🔁 Service Recovery

### 1️⃣ Restore Application Service

Run on VM-01:

- `sudo systemctl start nginx` — Restore NGINX service

Purpose:

- Brings the web service back online.


### 2️⃣ Validate Backend Rejoin

Run from local machine:

- `for i in {1..10}; do curl http://<LB_PUBLIC_IP>; done` — Confirm VM-01 re-enters traffic rotation

Expected:

- `OLD APP - Web Server 01`
- `NEW APP - Web Server 02`


### 3️⃣ Azure Portal Recovery Validation

Navigation:

- Azure Portal → Load Balancer → `web-lb`
- Monitoring → Insights

Expected:

- `web-vm-01` → Healthy green
- `web-vm-02` → Healthy green

Purpose:

- Confirms both backends are restored to active rotation.


## 🔧 Backend Operations / Migration Scenario

### 1️⃣ Attempt Backend Removal

Attempt to remove `web-vm-02` directly from `web-backend-pool`.

Observed:

- Azure blocks removal because the backend pool is still attached to `web-http-rule`.

Important:

- Azure enforces dependency order between rule, backend pool, NIC, and VM.


### 2️⃣ Detach Load Balancing Rule

Navigation:

- Load Balancer → Load balancing rules
- Edit `web-http-rule`
- Set Backend Pool = None
- Save

Purpose:

- Temporarily removes rule dependency from the backend pool.


### 3️⃣ Remove Backend VM

Navigation:

- Backend Pools → `web-backend-pool`
- Remove `web-vm-02`
- Save

Purpose:

- Simulates migration cutover or backend drain by removing one server from traffic rotation.


### 4️⃣ Reattach Load Balancing Rule

Navigation:

- Load balancing rules → `web-http-rule`
- Set Backend Pool = `web-backend-pool`
- Save

Purpose:

- Restores the frontend-to-backend routing path using the updated backend pool.


### 5️⃣ Validate Backend Removal

Run from local machine:

- `for i in {1..10}; do curl http://<LB_PUBLIC_IP>; done` — Confirm traffic only reaches remaining backend

Expected:

- `OLD APP - Web Server 01`

Purpose:

- Confirms `web-vm-02` is removed from active traffic rotation.


## ✅ Validation

### Confirmed

- Both VMs were accessible independently before Load Balancer deployment
- NGINX responded locally and externally
- Load Balancer distributed traffic across both backend VMs
- Azure Insights showed both backend VMs as healthy
- Health probe removed unhealthy backend after NGINX stopped
- Backend rejoined after NGINX restart
- Backend removal worked after detaching rule dependency


## 🧯 Lessons Learned

### Issues Encountered

- SSH failed when private key permissions were not set correctly
- VM not visible in backend pool when deployed into a different VNet
- Load Balancer appeared sticky due to hash-based behavior and caching
- Backend pool removal was blocked while tied to a load balancing rule
- Azure Insights sometimes lagged behind actual backend state


### Fixes Applied

- Used `chmod 400` to set key permissions correctly
- Rebuilt VM into the same VNet as the first backend VM
- Used no-cache curl testing to validate fresh responses
- Detached load balancing rule before modifying backend pool
- Restarted NGINX to restore backend health


### Key Takeaways

- Validate backend VMs before introducing a Load Balancer
- Backend pool members must be in the same VNet
- Load Balancer health probes control traffic eligibility
- VM running state does not guarantee application health
- Backend pools can be locked by rule dependencies
- Load balancing is hash-based, not strict round-robin
- Azure Insights may require refresh or time to reflect backend state


## 🚀 Future Improvements

- Blue/Green deployment using separate backend pools
- Azure Application Gateway for Layer 7 routing
- HTTPS with SSL termination
- VM Scale Sets for autoscaling
- Custom health probe endpoint such as `/health`
- NAT Gateway for controlled outbound access
- Terraform version of the build


## 🧹 Final Hygiene Cleanup

### Cleanup Performed

None during initial deployment.


### Recommended Cleanup

- Delete Load Balancer
- Delete Public IP
- Delete Virtual Machines
- Remove unused NICs
- Delete Resource Group if lab-only
