# ☁️ Azure App Service Deployment

Provisioned a managed Azure Web App using App Service to validate PaaS hosting, runtime detection, deployment workflows, networking defaults, cost awareness, and operational cleanup.


## 🏗️ Environment Build Choices

### Project Details

- **Subscription:** azurekmlprod  
- **Resource Group:** kml_rg_main-9eafd04f543b47e5  

### Instance Details

- **App Name:** lab-webapp  
- **Default Hostname:** Secure unique hostname enabled  
- **Publish Method:** Code  
- **Runtime Stack:** Node 24 LTS  
- **Operating System:** Linux  
- **Region:** East US  

### Pricing Plan

- **App Service Plan:** ASP-kmlrgmain9eafd04f543b47e5-91ed  
- **Tier:** Premium V3 (P0V3)  
- **vCPU:** 1  
- **Memory:** 4 GB  
- **Zone Redundancy:** Disabled  

### Database

- Not enabled during initial deployment

### Deployment Settings

- Continuous Deployment disabled during build  
- Basic Authentication disabled  

### Networking

- Public Access enabled  
- Virtual Network Integration disabled  

### Monitoring + Security

- Application Insights not enabled  
- Microsoft Defender not enabled (insufficient permissions)

### Tags

- None assigned during build


## 💻 Commands Used

- `chmod 400 az-lab-vm_key.pem` — Secure private key permissions  
- `ssh -i az-lab-vm_key.pem azureuser@public-ip` — Connect to VM using SSH  
- `sudo apt update -y` — Refresh package repositories  
- `sudo apt install nginx -y` — Install NGINX  
- `sudo systemctl enable nginx` — Enable on boot  
- `sudo systemctl start nginx` — Start service  
- `curl -I http://public-ip` — Validate HTTP response  
- `top` — View live resource usage

## ✅ Validation

- Azure Web App created successfully  
- App status = Running  
- Runtime status = Healthy  
- Node 24 LTS recognized automatically  
- Linux App Service Plan attached  
- Public Azure hostname generated  
- Inbound public access enabled  
- Outbound IP addresses assigned  
- Restart / Stop / Delete controls visible  
- Deployment Center available

## 🧠 Lessons Learned

- Azure App Service removes need to manage OS patching and server maintenance  
- Runtime stack selection is critical for successful application startup  
- Premium tiers can become expensive quickly for simple labs  
- Networking and monitoring can be layered on after deployment  
- Managed platforms still require cost governance  
- Review screen is valuable for catching incorrect SKU choices before launch  

## 🔮 Future Improvements

- Deploy via GitHub Actions CI/CD  
- Enable Application Insights monitoring  
- Configure custom domain + HTTPS certificate  
- Add deployment slots for blue/green releases  
- Integrate VNet for private backend access  
- Scale plan testing under load  
- Test lower-cost Basic / Free tiers for labs  

## 🧹 Final Hygiene Cleanup

- Stopped web app after validation  
- Deleted App Service resource  
- Deleted App Service Plan if no longer needed  
- Confirmed no leftover billable resources remained  
- Prevented unnecessary ongoing charges  
- Returned environment to clean state  
