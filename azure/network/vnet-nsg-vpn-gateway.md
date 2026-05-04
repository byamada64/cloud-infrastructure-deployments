# Azure VNet, NSG & VPN Gateway Deployment

Hands-on Azure networking build focused on segmented virtual networking, subnet-level security, inbound firewall controls, route table preparation, and VPN Gateway deployment.


## 🎯 Objective

Design and deploy an Azure network foundation using a segmented virtual network, dedicated Network Security Groups, subnet-level inbound controls, and a Route-Based VPN Gateway.

This build demonstrates core Azure networking concepts used in enterprise environments including:

- Network segmentation  
- Access control enforcement  
- Azure Policy constraints  
- Gateway dependencies  
- Post-build validation  


## 🏗️ Environment Build Choices

### Resources Built

- Virtual Network: `corp-vnet`
- VPN Gateway: `kk-vpn-gateway`
- Route Table: `corp-app-rt`


## 🌐 Core Network Design

A primary Azure VNet was created using a private RFC1918 address range and segmented into multiple functional subnets.

### Subnets Created

- `default` `/24`
- `Management` `/24`
- `DMZ` `/24`
- `App` `/24`
- `DB` `/24`
- `GatewaySubnet` `/27`


## 🔐 Network Security Groups Created

- `mgmt-nsg`
- `dmz-nsg`
- `app-nsg`
- `db-nsg`


## 🌍 Public IP Resources Created

- `corp-vpn-pip`
- `corp-vpn-pip02`


## 🧠 Architecture Choices

- Used `/24` subnet sizing for core workload tiers
- Used `/27` subnet sizing for the Azure gateway subnet
- Created separate NSGs per subnet tier
- Designed segmented network for security boundaries
- Enabled Active-Active VPN Gateway with dual Public IPs


## ⚠️ Constraints Encountered (Azure Policy)

Azure sandbox enforced:

- Name: `kk-vpn-gateway`
- SKU: `VpnGw1` or `VpnGw1AZ`
- Generation: `Generation1`
- VPN Type: `RouteBased`


## 🔁 Build Sequence (Critical Order)

### 1. Create Virtual Network

- Create `corp-vnet`
- Define address space
- Create all subnets (exclude GatewaySubnet initially if unsure)


### 2. Create Subnets

- Management
- DMZ
- App
- DB

Verify CIDR ranges and segmentation.


### 3. Create Network Security Groups

- `mgmt-nsg`
- `dmz-nsg`
- `app-nsg`
- `db-nsg`


### 4. Configure Firewall Rules

#### Management
- RDP (3389)
- SSH (22)

#### DMZ
- HTTP (80)
- HTTPS (443)

#### App
- Internal app ports only

#### DB
- DB ports from App subnet only


### 5. Associate NSGs to Subnets

- `mgmt-nsg` → Management
- `dmz-nsg` → DMZ
- `app-nsg` → App
- `db-nsg` → DB


### 6. Create Public IP Addresses

- `corp-vpn-pip`
- `corp-vpn-pip02`

Configuration:
- SKU: Standard
- Assignment: Static

---

### 7. Create Gateway Subnet (Critical)

- Name must be: `GatewaySubnet`
- CIDR: `/27`

⚠️ Required for VPN Gateway deployment


### 8. Deploy VPN Gateway

Configuration:

- Name: `kk-vpn-gateway`
- Gateway Type: VPN
- VPN Type: RouteBased
- SKU: `VpnGw1AZ`
- Generation: `Generation1`
- Active-Active: Enabled
- BGP: Enabled
- ASN: `65515`

Networking:
- VNet: `corp-vnet`
- Subnet: `GatewaySubnet`

Public IPs:
- Primary: `corp-vpn-pip`
- Secondary: `corp-vpn-pip02`

⏱️ Deployment time: ~30–45 minutes


### 9. Create Route Table

- Name: `corp-app-rt`
- Reserved for future routing scenarios


### 10. Validation & Monitoring

Verify:

- VPN Gateway deployed successfully
- Public IPs attached
- Metrics available
- Resource Visualizer reflects topology

Tools used:

- Network Watcher
- VPN diagnostics
- Metrics & Logs


## 🔐 Inbound Firewall Summary

| Subnet | Purpose | Rules |
|------|--------|------|
| Management | Admin access | RDP, SSH |
| DMZ | Public apps | HTTP, HTTPS |
| App | Internal apps | App ports |
| DB | Backend | DB ports (restricted) |


## 🔗 NSG Associations

- Management → `mgmt-nsg`
- DMZ → `dmz-nsg`
- App → `app-nsg`
- DB → `db-nsg`


## ✅ Validation

Confirmed:

- VNet and subnets created
- NSGs applied
- Public IPs created
- VPN Gateway deployed
- Metrics and diagnostics accessible


## 🧯 Lessons Learned

### Issues Encountered

- Gateway deployment failed due to:
  - Incorrect subnet name
  - Policy restrictions
  - Invalid SKU / Generation


### Fixes Applied

- Renamed subnet to `GatewaySubnet`
- Changed SKU to approved value
- Set Generation to `Generation1`
- Updated gateway name to match policy


### Key Takeaways

- `GatewaySubnet` is mandatory and name-sensitive
- Azure Policy can override configurations
- VPN Gateway deployments take time (~30+ min)
- Public IPs must exist before gateway deployment
- Always validate before deployment


## 🚀 Future Improvements

- Add Local Network Gateway
- Configure Site-to-Site VPN
- Add Azure Firewall
- Implement route table associations
- Convert to Terraform
- Automate with Ansible


## 🧹 Cleanup Recommendations

- Remove unused Public IPs
- Delete failed deployments
- Review quotas
- Remove unused NSGs
- Clean up test route tables
