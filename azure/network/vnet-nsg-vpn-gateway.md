# Azure VNet, NSG & VPN Gateway Deployment

Hands-on Azure networking build focused on segmented virtual networking, subnet-level security, inbound firewall controls, route table preparation, and VPN Gateway deployment.

---

## 🎯 Objective

Design and deploy an Azure network foundation using a segmented virtual network, dedicated Network Security Groups, subnet-level inbound controls, and a Route-Based VPN Gateway.

This build demonstrates core Azure networking concepts used in enterprise environments including network segmentation, access control, gateway dependencies, and post-build validation.

---

## 🏗️ Environment Build Choices

### Resources Built

- Virtual Network: `corp-vnet`
- VPN Gateway: `kk-vpn-gateway`
- Route Table: `corp-app-rt`

### Core Network Design

A primary Azure VNet was created using a private RFC1918 address range and segmented into multiple functional subnets.

### Subnets Created

- `default` `/24`
- `Management` `/24`
- `DMZ` `/24`
- `App` `/24`
- `DB` `/24`
- `GatewaySubnet` `/27`

### Network Security Groups Created

- `mgmt-nsg`
- `dmz-nsg`
- `app-nsg`
- `db-nsg`

### Public IP Resources Created

- `corp-vpn-pip`
- `corp-vpn-pip02`

### Architecture Choices

- Used `/24` subnet sizing for core workload tiers.
- Used `/27` subnet sizing for the Azure gateway subnet.
- Created separate NSGs for each major subnet tier.
- Used a segmented model to isolate management, DMZ, application, and database traffic zones.
- Prepared route table resources for future routing customization.

### Constraints Encountered

Azure sandbox policy required specific VPN Gateway settings:

- Name: `kk-vpn-gateway`
- Allowed SKU: `VpnGw1` / `VpnGw1AZ`
- Generation: `Generation1`
- VPN Type: `RouteBased`

---

## 💻 Commands Used

### Portal Actions

This build was completed primarily through the Azure Portal.

### Build Sequence Performed

- Created `corp-vnet`
- Defined address space
- Created workload subnets
- Created Network Security Groups
- Added inbound firewall rules
- Associated NSGs to subnets
- Created Public IP resources
- Created `GatewaySubnet`
- Deployed VPN Gateway
- Created route table
- Reviewed monitoring tools

### CLI / PowerShell / Bash

No CLI tools were used during this deployment.

### ARM / Template Review

Azure deployment details were reviewed from the Azure Resource Manager deployment page.

---

## 🔐 Inbound Firewall Rules Applied

### Management Subnet (`mgmt-nsg`)

Used for management access and administrative connectivity.

Typical inbound rules added:

- RDP (3389)
- SSH (22)
- Restricted source access where possible

### DMZ Subnet (`dmz-nsg`)

Used for internet-facing workloads.

Typical inbound rules added:

- HTTP (80)
- HTTPS (443)

### App Subnet (`app-nsg`)

Used for internal application workloads.

Typical inbound rules added:

- App-specific internal ports
- Restricted subnet-to-subnet traffic

### DB Subnet (`db-nsg`)

Used for backend database workloads.

Typical inbound rules added:

- Database ports from approved application subnet only
- No broad internet-facing access

---

## 🔗 NSG Association to Subnets

After rule creation, NSGs were associated to their matching subnet tiers:

- `mgmt-nsg` → Management
- `dmz-nsg` → DMZ
- `app-nsg` → App
- `db-nsg` → DB

This provided subnet-level traffic control and separation between workloads.

---

## ✅ Validation

### Deployment Success

Confirmed the following resources were created:

- `corp-vnet`
- All planned subnets
- NSGs
- Public IPs
- Route Table
- `kk-vpn-gateway`

### Operational Checks

Reviewed:

- Network Watcher Center
- VPN troubleshoot blade
- Usage + quotas
- Diagnostic logs
- Resource inventory
- VPN Gateway metrics

### Final State

The Azure VPN Gateway completed deployment successfully and was visible in the portal with metrics available.

---

## 🧯 Lessons Learned

### Issues Encountered

VPN Gateway deployment initially failed because the reserved subnet requirement was not met.

Azure required the subnet to be named exactly:

`GatewaySubnet`

Temporary retriable errors were also encountered while Azure networking resources were updating.

### Fixes Applied

- Removed incorrectly named gateway subnet
- Recreated subnet as `GatewaySubnet`
- Confirmed gateway subnet during review stage
- Re-ran gateway deployment successfully

### Key Takeaways

- Build the VNet and subnet structure first.
- Apply NSGs after subnet creation.
- Create firewall rules before association where practical.
- Azure VPN Gateway requires a dedicated `GatewaySubnet`.
- `/27` is a strong subnet size for gateway deployments.
- Network Watcher is useful for post-build validation.

---

## 🚀 Future Improvements

- Add Application Gateway
- Add Local Network Gateway
- Add site-to-site VPN testing
- Add route table associations
- Add Azure Firewall
- Convert build to Terraform
- Convert build to Ansible

---

## 🧹 Final Hygiene Cleanup

### Cleanup Performed

No cleanup was performed during the initial session.

### Recommended Cleanup

- Remove unused public IP resources
- Remove failed deployment remnants
- Review quota recovery
- Remove unused NSGs
- Delete temporary route resources if not needed
