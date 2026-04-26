# 🛠️ Ansible Linux Baseline Configuration

## 🏗️ Environment Build Choices

- Platform: KodeKloud Ansible Sandbox Lab
- Ansible Version: 2.18.x
- Control Node: ansible-controller
- Managed Nodes: web1, web2
- Operating System: Ubuntu Linux
- Services Configured: User Management, MOTD Banner, NGINX
- Purpose: Standardize new Linux servers with a repeatable baseline configuration

## 📄 YAML File Used

### baseline.yml

~~~yaml
---
- name: Baseline Linux Server Configuration
  hosts: webservers
  become: yes

  tasks:

    - name: Ensure opsadmin user exists
      user:
        name: opsadmin
        state: present
        shell: /bin/bash

    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Ensure nginx is running and enabled
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Deploy managed MOTD banner
      copy:
        dest: /etc/motd
        content: |
          Authorized Access Only
          Managed by Ansible
          Host: {{ inventory_hostname }}

    - name: Validate nginx status
      command: systemctl is-active nginx
      register: nginx_status
      changed_when: false

    - name: Show nginx result
      debug:
        msg: "NGINX on {{ inventory_hostname }} is {{ nginx_status.stdout }}"
~~~

## 💻 Commands Used

- `mkdir -p ~/ansible-lab`
- `cd ~/ansible-lab`
- `vi baseline.yml`
- `ansible-playbook -i hosts baseline.yml`
- `ansible webservers -i hosts -m shell -a "cat /etc/motd"`
- `ansible-playbook -i hosts baseline.yml`

## ✅ Validation

- Created `opsadmin` user on both managed nodes
- Updated apt package cache successfully
- Verified NGINX package installed on:
  - web1
  - web2
- Confirmed NGINX service is started and enabled
- Deployed managed `/etc/motd` banner to both servers
- Verified hostname variable rendered dynamically:
  - web1
  - web2
- Confirmed service validation returned `active`
- Re-ran playbook and confirmed idempotency with `changed=0`

## 🧠 Lessons Learned

- Baseline playbooks reduce manual server setup time
- User creation can be standardized across all hosts
- MOTD banners help identify managed systems quickly
- Service validation improves deployment confidence
- Dynamic variables allow host-specific messaging
- Idempotent playbooks are ideal for recurring compliance checks
- Ansible can combine account management, packages, services, and files in one workflow

## 🔮 Future Improvements

- Add SSH hardening configuration
- Add sudo access for `opsadmin`
- Install common admin tools (`curl`, `vim`, `net-tools`)
- Add firewall baseline rules
- Enforce password rotation policies
- Convert baseline tasks into reusable Ansible roles
- Integrate with CI/CD for new server onboarding

## 🧹 Final Hygiene Cleanup

- Remove temporary lab users if environment resets
- Revert MOTD banner if required
- Remove unused packages after testing
- Sanitize inventory files before GitHub commits
- Do not commit plaintext credentials
- End temporary KodeKloud sandbox when finished
