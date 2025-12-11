# handlers

* Handlers are special tasks in Ansible that run only when notified by another task.

* They are mainly used for actions like restarting services, reloading configs, etc.

* Key points:

  * Handlers run only when something changes.

  * They are triggered using:

    * **notify: \<handler_name>**

* All handlers run at the end of the play (not immediately).

* Handlers with the same name run only once, even if notified multiple times.

**Example:**

```yaml
---
- hosts: all
  become: yes

  tasks:
    - name: Copy Apache config file
      copy:
        src: apache.conf
        dest: /etc/httpd/conf/httpd.conf
      notify: restart apache

  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```