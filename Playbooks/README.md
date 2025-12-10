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

### ansible-playbook command with options

* Basic Syntax
  
  * **ansible-playbook \<playbook-name>**

    Example:  
    **Bansible-playbook playbook.yml**

    * default inventory: /etc/ansible/hosts

* Run with a Specific Inventory (-i)

  * **ansible-playbook -i \<inventory_name> \<playbook-name>**

    Example:  
    **ansible-playbook -i inventory.ini playbook.yml**

* Run Playbook on Specific Hosts (--limit)

  * **ansible-playbook playbook.yml --limit \<host-name>**

  Example:  
  **ansible-playbook playbook.yml --limit web1**

  * **ansible-playbook playbook.yml --limit \<group-name>**

  Example:  
  **ansible-playbook playbook.yml --limit webservers**

* Run as a Particular User (-u)

  * **ansible-playbook playbook.yml -u ec2-user**

* Check Mode (Dry Run) (--check)

  * Shows what would change, but does not apply changes.

  * **ansible-playbook playbook.yml --check**

* Diff Mode (See File Changes) (--diff)

  * **ansible-playbook playbook.yml --diff**

* Run Only Tagged Tasks (--tags)

  * **ansible-playbook playbook.yml --tags install**

* Skip Tagged Tasks (--skip-tags)

  * **ansible-playbook playbook.yml --skip-tags config**