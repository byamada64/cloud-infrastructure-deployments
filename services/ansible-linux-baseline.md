# 🛠️ Ansible Linux Baseline Configuration

## 1️⃣ What it is

Ansible Linux Baseline Configuration is an automation workflow used to standardize newly deployed or existing Linux servers with a consistent starting configuration.

This typically includes:

- Creating standard administrative users
- Updating package repositories
- Installing required software
- Enabling core services
- Applying security or compliance banners
- Verifying service health
- Reducing manual setup steps

In this build, Ansible was used to create an `opsadmin` account, install NGINX, configure a managed MOTD banner, and validate service status across multiple Linux hosts.

## 2️⃣ Why companies use it

Organizations use baseline automation because manually configuring servers creates inconsistency and wasted time.

Companies commonly use Linux baseline playbooks for:

- Faster server onboarding
- Standardized operating environments
- Reduced manual build effort
- Consistent admin account creation
- Faster deployment readiness
- Easier compliance enforcement
- Repeatable infrastructure standards
- Lower configuration drift risk

This helps operations teams deploy clean and predictable server environments.

## 3️⃣ What problem it solves

Without a baseline process, servers may be built differently depending on who configured them.

That can create issues such as:

- Missing admin accounts
- Different installed packages
- Services not enabled
- Inconsistent login banners
- Slower troubleshooting
- Harder audits
- Increased setup time
- Configuration drift across environments

Ansible solves this by applying the same YAML-defined standard to every target host.

## 4️⃣ Day-to-day engineer relevance

Baseline automation is highly relevant for infrastructure and operations engineers because new systems are constantly being added, rebuilt, or refreshed.

Daily uses include:

- Preparing Linux servers for production
- Adding operations accounts
- Installing required tools
- Enabling monitoring prerequisites
- Applying company standards
- Validating critical services
- Reapplying standards during audits
- Supporting rapid onboarding requests

It reduces repetitive setup work and improves consistency.

## 5️⃣ When to choose it over a VM

A VM only provides the compute resource and operating system.

Choose a Linux baseline automation workflow when the VM still needs post-build configuration such as:

- User creation
- Package installs
- Service enablement
- Login banners
- Security settings
- Operational readiness checks

A common enterprise workflow is:

- Cloud team provisions the VM
- Ansible baseline configures the server
- Application team deploys workloads

## 6️⃣ Real-world use cases

- Build-ready Linux servers after VMware deployment
- Standardize Azure or AWS Linux VMs after provisioning
- Create admin accounts for operations teams
- Install NGINX or utility packages
- Apply `/etc/motd` legal banners
- Enable services automatically on boot
- Validate server readiness before handoff
- Reapply standards during quarterly audits
- Support mass onboarding projects

## 7️⃣ Related builds

- [Ansible NGINX Deployment](../ci-cd/ansible-nginx-deployment.md)
- [Ansible Linux Health Audit](../ci-cd/ansible-health-audit.md)
- [Ansible Daily Linux Operations Check](../ci-cd/ansible-daily-ops.md)
