# PLaybooks

* A Playbook in Ansible is a YAML file where you define what tasks should be executed on hosts. 

* It’s essentially a set of instructions that tells Ansible how to configure your systems, deploy applications, or manage infrastructure.

### Basic Structure of a Playbook

```yaml
---
- name: Install Apache on web servers
  hosts: webservers
  become: yes

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd service
      service:
        name: httpd
        state: started
        enabled: yes
```

* **Explanation of fields:**

  * **- name:**

    * A human-readable description of what the playbook or task does.

  * **hosts:**

    * Specifies which hosts or groups in your inventory the playbook should run on.
    
    * Example: webservers, dbservers, or all.

  * **become:**

    * If set to yes, the task runs with sudo privileges.

  * **vars:**
    
    * Variables that can be used in tasks.

  * **tasks:**

    * A list of tasks to perform. Each task usually calls an Ansible module like yum, copy, file, service, etc.