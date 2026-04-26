# ☸️ Azure Kubernetes Service (AKS) Deployment

Provisioned and validated an Azure Kubernetes Service (AKS) cluster to demonstrate managed Kubernetes operations, containerized workload deployment, public load balancing, rolling updates, horizontal scaling, and cloud-native troubleshooting within a governed Azure sandbox environment.


# 🏗️ Environment Build Choices

## Project Details

**Subscription:** Existing assigned lab subscription provided by training environment  
**Resource Group:** Existing approved Resource Group provided by sandbox tenant

## Notes

- Manual AKS creation attempts were initially blocked by policy restrictions
- Successful deployment path used **AKS Automatic Cluster**
- Existing subscription and Resource Group were required due to sandbox permissions
- Governed tenants often restrict VM sizes, add-ons, monitoring, or custom networking

## Cluster Details

**Cluster Name:** `auto-lab-01`  
**Region:** East US  
**Kubernetes Version:** `1.34.4`  
**Pricing Tier:** Standard  
**SKU:** Base

## Node Configuration

**Node Pools:** 1  
**Node Size:** `Standard_D2s_v3`  
**OS Image:** Microsoft Azure Linux 3.0  
**Container Runtime:** containerd

## Networking

**Network Model:** Azure CNI Overlay  
**Dataplane:** Cilium  
**Network Policy Engine:** Cilium  
**Load Balancer:** Standard  
**Private Cluster:** Disabled  
**Authorized IP Ranges:** Disabled

## Security

**Identity:** System-assigned managed identity  
**Authentication:** Local accounts with Kubernetes RBAC  
**Encryption:** Platform-managed at-rest encryption

## Monitoring

**Container Logs:** Disabled during initial build  
**Prometheus / Grafana:** Optional add-ons later tested but blocked by sandbox RBAC  
**Alerts:** Disabled


# 💻 Commands Used

- `az aks get-credentials --resource-group kml_rg_main-70166347af824eaa --name auto-lab-01 --overwrite-existing`  
  Connect local kubeconfig to the AKS cluster

- `kubectl get nodes`  
  Validate node registration and readiness

- `kubectl get pods -A`  
  Review all namespaces and system workloads

- `kubectl cluster-info`  
  Confirm Kubernetes control plane connectivity

- `kubectl create deployment nginx --image=nginx`  
  Deploy NGINX test workload

- `kubectl get deployments`  
  Confirm deployment creation

- `kubectl get pods`  
  Verify pod startup status

- `kubectl expose deployment nginx --port=80 --type=LoadBalancer`  
  Publish workload through Azure public load balancer

- `kubectl get svc -w`  
  Watch service until external IP assignment completed

- `curl http://135.237.27.95`  
  Validate external HTTP connectivity

- `kubectl scale deployment nginx --replicas=3`  
  Scale workload horizontally to three replicas

- `kubectl set image deployment/nginx nginx=nginx:latest`  
  Perform rolling image update

- `kubectl rollout status deployment/nginx`  
  Validate successful rollout completion

- `kubectl scale deployment nginx --replicas=5`  
  Scale workload to five replicas

- `kubectl create deployment apache --image=httpd`  
  Deploy second containerized workload

- `kubectl describe deployment nginx`  
  Inspect deployment configuration and events

- `kubectl get events --sort-by=.metadata.creationTimestamp`  
  Review cluster event timeline

- `kubectl delete svc nginx`  
  Remove public service endpoint

- `kubectl delete deployment nginx`  
  Delete NGINX workload

- `kubectl delete deployment apache`  
  Delete Apache workload

- `az aks delete --resource-group kml_rg_main-70166347af824eaa --name auto-lab-01 --yes --no-wait`  
  Initiate cluster teardown


# ✅ Validation

## Cluster Validation

- AKS cluster deployed successfully
- Kubernetes API reachable through `kubectl`
- Worker node status confirmed **Ready**
- Core system services healthy (`coredns`, `metrics-server`, `cilium`, `azure-policy`)

## Application Validation

- NGINX deployment created successfully
- Apache deployment created successfully
- Pod replicas scaled from **1 → 3 → 5**
- Rolling image update completed successfully
- All replicas reported healthy and available

## Networking Validation

- Azure LoadBalancer public IP assigned successfully
- External browser access reached NGINX welcome page
- End-to-end traffic flow validated from internet to pod

## Operational Validation

- Cluster metrics visible in Azure portal
- Node resource utilization visible
- Kubernetes events confirmed scheduling and rollout activity


# 🧠 Lessons Learned

- AKS Automatic mode can succeed where manual builds fail in restricted tenants
- Azure sandbox environments may enforce hidden policies on sizing and add-ons
- `LoadBalancer` services provision public IPs asynchronously
- `kubectl get events` is highly effective for troubleshooting deployments
- Rolling updates allow zero-downtime workload changes
- Horizontal scaling is fast and efficient with managed Kubernetes
- Browser validation provides stronger proof than CLI output alone
- Troubleshooting persistence is often more valuable than perfect first-pass deployment


# 🔮 Future Improvements

- Enable Managed Prometheus and Grafana in unrestricted tenant
- Integrate Azure Container Registry (ACR)
- Deploy Ingress Controller with DNS hostname routing
- Configure Horizontal Pod Autoscaler (HPA)
- Create multiple namespaces for workload separation
- Add persistent storage using Azure Disk / Azure Files
- Implement private AKS cluster model
- Automate cluster builds using Terraform or Bicep
- Add CI/CD pipeline deployment testing


# 🧹 Final Hygiene Cleanup

- Deleted NGINX service after validation
- Deleted NGINX deployment after testing
- Deleted Apache deployment after testing
- Confirmed no remaining workloads in default namespace
- Initiated AKS cluster deletion
- Left no unnecessary billable lab resources running
