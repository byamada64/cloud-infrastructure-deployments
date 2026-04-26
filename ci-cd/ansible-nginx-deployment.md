# 🚀 Ansible NGINX Deployment

## 🏗️ Environment Build Choices

- Platform: KodeKloud Ansible Sandbox Lab
- Ansible Version: 2.18.x
- Control Node: ansible-controller
- Managed Nodes: web1, web2
- Operating System: Ubuntu Linux
- Web Service: NGINX
- Purpose: Automate multi-server web deployment using Ansible playbooks

## 📄 YAML File Used

### deploy.yml

~~~yaml
---
- name: Install and Deploy Web Server
  hosts: webservers
  become: yes

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Deploy homepage
      copy:
        dest: /var/www/html/index.html
        content: |
          <h1>Ansible Deployment Success</h1>
          <p>Server: {{ inventory_hostname }}</p>
~~~

## 💻 Commands Used

- `mkdir -p ~/ansible-lab`
- `cd ~/ansible-lab`
- `vi hosts`
- `vi deploy.yml`
- `ansible webservers -i hosts -m ping`
- `ansible-playbook -i hosts deploy.yml`
- `ansible webservers -i hosts -m shell -a "cat /var/www/html/index.html"`
- `ansible-playbook -i hosts deploy.yml`

## ✅ Validation

- Successfully connected to both managed hosts using the Ansible inventory file
- Installed NGINX on both managed nodes:
  - web1
  - web2
- Started and enabled the NGINX service on both servers
- Deployed a custom homepage using the Ansible `copy` module
- Verified dynamic hostname output using `{{ inventory_hostname }}`
- Confirmed homepage content rendered correctly on both hosts
- Re-ran the playbook and confirmed idempotency with `changed=0`
- Final play recap completed with:
  - `unreachable=0`
  - `failed=0`

## 🧠 Lessons Learned

- Ansible can deploy applications to multiple Linux servers simultaneously
- Inventory files simplify multi-host management
- Playbooks create repeatable deployment workflows
- The `copy` module can distribute static configuration or web content
- The `service` module enforces desired runtime state for applications
- Variables such as `{{ inventory_hostname }}` allow dynamic per-host customization
- Idempotency prevents duplicate or unnecessary changes on repeated playbook runs

## 🔮 Future Improvements

- Replace inline `copy` content with a Jinja2 template
- Add an NGINX handler to restart the service only when configuration changes
- Add HTTP validation using `curl`
- Add firewall validation for HTTP/HTTPS access
- Convert the playbook into a reusable Ansible role
- Integrate the playbook with Jenkins for CI/CD-driven deployment testing

## 🧹 Final Hygiene Cleanup

- Stop NGINX on managed hosts if the lab requires cleanup
- Remove NGINX packages if resetting the environment manually
- Remove temporary test homepage files if no longer needed
- Sanitize inventory files before committing to GitHub
- Do not commit plaintext passwords or lab credentials
- End the temporary KodeKloud sandbox when finished
