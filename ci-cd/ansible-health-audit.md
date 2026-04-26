# 🩺 Ansible Linux Health Audit

## 🏗️ Environment Build Choices

- Platform: KodeKloud Ansible Sandbox Lab
- Ansible Version: 2.18.x
- Control Node: ansible-controller
- Managed Nodes: web1, web2
- Operating System: Ubuntu Linux
- Audit Scope: Kernel, Disk Usage, CPU Processes, Memory Processes
- Purpose: Run operational health checks across multiple Linux servers using Ansible

## 📄 YAML File Used

### health-audit.yml

~~~yaml
---
- name: Linux Health Audit
  hosts: webservers
  become: yes

  tasks:
    - name: Show kernel version
      command: uname -r
      register: kernel_version
      changed_when: false

    - name: Show disk utilization
      command: df -h
      register: disk_usage
      changed_when: false

    - name: Show top CPU processes
      shell: ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -10
      register: top_cpu
      changed_when: false

    - name: Show top memory processes
      shell: ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -10
      register: top_mem
      changed_when: false

    - name: Display health audit summary
      debug:
        msg:
          - "Host: {{ inventory_hostname }}"
          - "Kernel: {{ kernel_version.stdout }}"
          - "Disk Usage:"
          - "{{ disk_usage.stdout_lines }}"
          - "Top CPU Processes:"
          - "{{ top_cpu.stdout_lines }}"
          - "Top Memory Processes:"
          - "{{ top_mem.stdout_lines }}"
~~~

## 💻 Commands Used

- `mkdir -p ~/ansible-lab`
- `cd ~/ansible-lab`
- `vi health-audit.yml`
- `ansible-playbook -i hosts health-audit.yml`

## ✅ Validation

- Successfully connected to all managed hosts
- Collected kernel version from:
  - web1
  - web2
- Verified disk utilization on both servers
- Retrieved top CPU-consuming processes
- Retrieved top memory-consuming processes
- Displayed structured health summaries per host
- Final play recap completed with:
  - `unreachable=0`
  - `failed=0`

## 🧠 Lessons Learned

- Ansible can execute read-only audits across many servers quickly
- `changed_when: false` keeps reporting clean for audit tasks
- Operational visibility improves when system data is centralized
- Kernel version checks help validate patch levels
- Disk usage audits can identify capacity risks early
- Process analysis helps spot resource-heavy workloads
- Health checks can be automated without changing host state

## 🔮 Future Improvements

- Add filesystem usage alert threshold (80%+)
- Add memory utilization percentage calculations
- Capture load average and uptime
- Export results to CSV or HTML reports
- Email audit summaries automatically
- Schedule recurring audits through Jenkins or cron
- Convert into reusable audit role for Linux fleets

## 🧹 Final Hygiene Cleanup

- Remove temporary audit files if exported locally
- Sanitize inventory files before GitHub commits
- Do not commit plaintext credentials
- Clear terminal history if needed in shared labs
- End temporary KodeKloud sandbox when finished
