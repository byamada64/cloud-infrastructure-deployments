# ☁️ Azure VM + Storage + Container + Private Endpoint Lab

## 🎯 Objective

Provisioned a repeatable multi-service Azure environment to validate compute deployment, secure storage access, private networking, governance tagging, container workloads, remote administration, and lifecycle cleanup awareness.


## 🏗️ Environment Build Choices

### 🏷️ Resource Group / Networking

- **Resource Group:** Existing lab resource group reused
- **Virtual Network:** Default Azure lab VNet used
- **Subnet:** Default subnet selected
- **Public IP:** Enabled temporarily for testing
- **Purpose:** Fast lab deployment with minimal custom networking
- **Enterprise Note:** Production environments may require dedicated VNets, subnet planning, NSGs, routing controls, private DNS, firewall review, and formal network requests.

### 🖥️ VM

- **VM Name:** `az-lab-vm`
- **Region:** East US
- **Availability Zone:** Zone-aware deployment enabled
- **Operating System:** Windows Server Datacenter (2022 / 2025 lab image)
- **VM Size:** Standard D2s_v3
- **Security Type:** Trusted Launch
- **Secure Boot:** Enabled
- **vTPM:** Enabled
- **Public RDP Access:** Enabled temporarily for testing
- **Access Method:** Microsoft Remote Desktop from macOS
- **Purpose:** Administrative access and validation testing

### 📦 Storage

- **Storage Account Name:** `bypepstorage01`
- **Region:** East US
- **Performance Tier:** Standard
- **Account Type:** GPv2
- **Redundancy:** LRS
- **Blob Services:** Enabled
- **Public Access:** Enabled during initial testing
- **Purpose:** Blob storage, SAS testing, private connectivity

### 🗂️ Blob Container

- **Container Type:** Blob container
- **Validation File:** `index_01.html`
- **Access Method:** SAS URL generated for controlled temporary access
- **Purpose:** Static file validation and object access testing

### 🐳 Containers

- **Deployment Type:** Azure Container Instance
- **Container Name:** `by-nginx01`
- **Image Source:** Quickstart Images
- **Image Used:** `nginx`
- **Region:** East US
- **CPU:** 1 vCPU
- **Memory:** 1.5 GB
- **Public Endpoint:** Enabled
- **Port:** 80
- **Purpose:** Lightweight web workload validation

### 🔒 Private Endpoint (PEP)

- **Resource Type:** Storage Account
- **Target Resource:** `bypepstorage01`
- **Subresource:** `blob`
- **Region:** East US
- **Purpose:** Private network path to Blob Storage

### 🏷️ Governance / Enterprise Awareness

- Applied **Tags** for:
  - Accounting
  - Billing
  - Ownership
  - Environment
  - Lifecycle / Lab Use
- Reused existing Resource Group
- Cost-aware sizing used where possible
- Demonstrates enterprise resource management practices


## 💻 Commands Used

### 🖥️ VM / Azure CLI

- `az vm create`
- `az vm show`
- `az vm list`
- `az vm open-port --port 3389`

### 📦 Storage / Azure CLI

- `az storage account create`
- `az storage account list`
- `az storage container create`
- `az storage blob upload`
- `az storage blob list`

### 🔑 SAS / Blob Access

- `az storage blob generate-sas`
- `az storage blob url`

### 🐳 Containers

- `az container create`
- `az container show`
- `az container list`
- `az container logs`

### 🔒 Private Endpoint (PEP)

- `az network private-endpoint create`
- `az network private-endpoint list`
- `az network private-dns zone list`

### 🖥️ Windows CMD Validation (Inside VM)

- `nslookup <storage-account-name>.blob.core.windows.net`
- `ping <target-host>` *(if allowed)*
- `ipconfig`
- `hostname`

### 🧹 Cleanup

- `az resource list --resource-group <resource-group-name>`
- `az vm delete`
- `az container delete`
- `az group delete`

## ✅ Validation

### 🖥️ VM

- VM deployed successfully in East US
- Public IP assigned successfully
- Microsoft Remote Desktop configured on macOS
- RDP login successful to Azure VM
- Administrative access confirmed

### 📦 Storage

- Storage account provisioned successfully
- Blob services enabled
- Container created successfully

### 🗂️ Blob / SAS Testing

- Uploaded validation file: `index_01.html`
- Generated SAS URL successfully
- Confirmed temporary browser access to hosted file
- Validated object retrieval workflow

### 🐳 Containers

- Azure Container Instance deployed successfully
- NGINX container started successfully
- Public endpoint responded on port 80
- Confirmed lightweight web workload availability

### 🔒 Private Endpoint (PEP)

- Private Endpoint created successfully
- Attached to correct storage account
- Blob subresource selected successfully
- Confirmed secure private connectivity design path

### 🌐 Name Resolution Testing

- Used `nslookup` from Windows VM
- Confirmed DNS resolution behavior for storage endpoint
- Validated networking awareness during testing


## 📘 Lessons Learned

### 🖥️ VM

- Successful deployment does not guarantee access; ports, credentials, and client setup must also be validated.
- Trusted Launch adds strong baseline security controls.
- Cross-platform management from macOS to Azure Windows VMs is practical and efficient.

### 📦 Storage

- GPv2 accounts offer modern flexible storage options.
- LRS is cost-efficient for labs and non-critical testing.
- Public access may help testing but should be tightened later.

### 🗂️ Blob / SAS

- SAS URLs are useful for controlled temporary access.
- Safer than broadly exposing storage publicly.
- Static HTML uploads provide simple functional testing.

### 🐳 Containers

- Azure Container Instances are excellent for rapid temporary workloads.
- Great option for testing without managing full VM infrastructure.
- Ideal for demos, validation, and lightweight services.

### 🔒 Private Endpoint (PEP)

- Private Endpoints improve security posture by reducing public exposure.
- Correct subresource selection matters (`blob`, `file`, `queue`, etc.).
- DNS behavior should be validated during private access testing.

### 🌐 Networking

- Default VNets and subnets are effective for labs and rapid builds.
- Enterprise environments often require pre-planned network architecture and approvals.
- Networking choices impact later integrations such as PEP, NSGs, and DNS.

### 🏷️ Governance

- Tags help accounting, billing, ownership, automation, and cleanup.
- Strong tagging habits scale well in enterprise environments.

### 🧹 Operations

- Multi-service builds create dependencies that affect cleanup order.
- Temporary subscriptions may expire before cleanup completes.


## 🚀 Future Improvements

- Disable public storage access after testing
- Restrict RDP to trusted IP ranges only
- Add NSG hardening rules
- Integrate Private DNS Zones
- Deploy containers privately
- Use Terraform or Bicep for repeatable builds
- Add Azure Monitor / Log Analytics
- Automate tag policy enforcement


## 🧹 Final Hygiene Cleanup

- Reviewed temporary resources before expiration
- Attempted cleanup of lab services
- Confirmed dependency awareness across resources
- Used cost-conscious temporary sizing
- Preserved build notes for future rebuilds and automation

