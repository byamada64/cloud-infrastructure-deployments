# 🛠️ Ansible Daily Linux Operations Check

## 1️⃣ What it is

Ansible Daily Linux Operations Check is an automation workflow used to perform common day-to-day operational validation tasks across Linux servers from a centralized control node.

Instead of manually logging into each host, engineers can run one playbook to gather system status across multiple servers.

This build focused on:

- Uptime checks
- Disk utilization review
- Memory usage review
- Failed systemd service detection
- NGINX service status validation
- Structured multi-host reporting

The playbook was designed as a repeatable read-only operations check.

## 2️⃣ Why companies use it

Organizations use daily operational checks to improve reliability, consistency, and early issue detection.

Common benefits include:

- Faster morning system reviews
- Reduced manual SSH effort
- Consistent operational procedures
- Faster service outage detection
- Improved visibility across servers
- Reduced missed failures
- Better maintenance readiness
- Standardized reporting for teams

This helps operations teams stay proactive rather than reactive.

## 3️⃣ What problem it solves

Manual daily checks across multiple systems can waste time and create inconsistency.

Common challenges include:

- Engineers checking servers differently
- Missed failed services
- Unknown disk growth issues
- Unnoticed memory pressure
- Slow response to service outages
- Repetitive manual login tasks
- Poor operational handoff visibility

Ansible solves this by executing the same health review across all managed nodes in one run.

## 4️⃣ Day-to-day engineer relevance

This is highly relevant for infrastructure, cloud operations, NOC, and systems administration teams.

Daily engineer uses include:

- Morning health checks
- Reviewing uptime after maintenance
- Checking NGINX or application services
- Reviewing disk usage trends
- Checking failed services after patching
- Confirming systems are stable before business hours
- Gathering quick status during incidents
- Supporting shift handoff reviews

This creates consistent operations discipline.

## 5️⃣ When to choose it over a VM

A VM only provides the compute platform.

Choose a daily operations workflow when the server already exists and requires recurring health validation.

Examples:

- Confirm production systems are healthy
- Review systems after reboots
- Validate services after changes
- Check resource usage trends
- Support operations teams with rapid status data

A common workflow is:

- VM already running
- Engineer runs Ansible ops check
- Findings drive remediation actions if needed

## 6️⃣ Real-world use cases

- Daily morning Linux fleet checks
- Validate web servers before traffic peaks
- Post-maintenance service verification
- Identify failed systemd units quickly
- Confirm disk capacity before incidents occur
- Review memory pressure on application servers
- Multi-server handoff reporting
- Rapid status collection during outages
- Support NOC operational runbooks

## 7️⃣ Related builds

- [Ansible NGINX Deployment](../ci-cd/ansible-nginx-deployment.md)
- [Ansible Linux Baseline Configuration](../ci-cd/ansible-linux-baseline.md)
- [Ansible Linux Health Audit](../ci-cd/ansible-health-audit.md)
