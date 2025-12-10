# ansible_facts

* ansible_facts are system information automatically collected by Ansible about managed hosts.

* Think of facts as metadata about a server.

* Examples:

  * OS type & version

  * IP address

  * CPU count

  * Memory

  * Disk

  * Hostname

* **setup** moudle is used to gather all facts

* These facts are stored in the variable: **ansible_facts**.

* we can access this data in the **ansible_facts** variable.

* To gather the facts

  **ansible \<hostname> -m setup**

  **ansible \<hostname> -m ansible.builtin.setup**

### Disabling facts

* By default ansible gathers facts at the beggining of each play.

* If we do not need to gather facts, we can turn off fact gathering at play level to improve scalability.

* Example:  

```yaml
- name: Testplay
  hosts: all
  gather_facts: false
```

**Example: In which using ansible_facts**

---
- name: Play1
  hosts: all
  tasks:
  - name: install the packages in ubuntu
    become: true
    apt:
      name: git
      state: present
    when: ansible_facts['distribution']=="Ubuntu"
