# Block

* **block** is used to group multiple tasks together.

* You can apply:

  * Common **when** condition

  * Common **tags**

  * Common **become**

  * Error handling using **rescue**

  * Always-run tasks using **always**

* It makes your playbook cleaner and easier to manage.

**Example 1:**

```yaml
- name: Basic block example
  hosts: all
  tasks:
    - block:
        - name: Create directory
          file:
            path: /tmp/myapp
            state: directory

        - name: Copy config file
          copy:
            src: config.ini
            dest: /tmp/myapp/config.ini
      become: true
      when: ansible_os_family == "RedHat"
      tags: 
        - setup
```
* In the above example: 

  * Both tasks run only if OS is RedHat.

  * Both tasks share the tag setup.

**Example 2:**

```yaml
- name: Block with rescue and always
  hosts: all
  tasks:
    - block:
        - name: Install nginx from repo
          package:
            name: nginx
            state: present

      rescue:
        - name: Fallback install using local rpm
          yum:
            name: /tmp/nginx.rpm
            state: present

      always:
        - name: Always run this task
          debug:
            msg: "Installation attempt completed"
```
* In the above Example:

  * **block** -> Try installation from repo

  * **rescue** -> If block fails, this runs

  * **always** -> Runs whether block succeeds or fails