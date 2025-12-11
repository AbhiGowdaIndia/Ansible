# Tags 

* Tags allow you to run only specific tasks in a playbook instead of running the whole play.

* Example:

  * Run only installation tasks

  * Run only configuration tasks

  * Skip a particular task

  * Speed up testing

**Example:**

```yaml
---
- hosts: all
  become: yes

  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present
      tags: install

    - name: Configure Apache
      template:
        src: httpd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      tags: config

    - name: Start Apache service
      service:
        name: httpd
        state: started
      tags: service
```

* Run only tasks with tag **install**:

  **ansible-playbook site.yml -t install**

* **Output:**

  * Only the "Install Apache" task runs

  * Other tasks (config, service) are skipped

* Run multiple tags:

  **ansible-playbook site.yml -t "install,config"**

* Runs:

  * **Install Apache**

  * **Configure Apache**

* Skips:

  * **Start Apache**

* Run all tasks except a tag

  * Use --skip-tags

    **ansible-playbook site.yml --skip-tags config**

  * This runs everything except the configuration task.
