# Ansible Modules

### The module which we commonly used in ansible playbook / roles with examples.

### File & Directory Management

* **copy**

  * Copy files from control node → managed nodes

  ```yaml
  - name: Copy app config
    copy:
      src: app.conf
      dest: /etc/myapp/app.conf
      owner: root
      mode: '0644'

* **template**

  * Copy files with Jinja2 variables

  ```yaml
  - name: Deploy nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf

* **file**

  * Create/delete files & directories, set permissions

  ```yaml
  - name: Create app directory
    file:
      path: /opt/myapp
      state: directory
      mode: '0755'

* **stat**

  * get facts about files, directories, or symbolic links on the target machine.**

  ```yaml
  - name: Check if /tmp/demo.txt exists
    stat:
      path: /tmp/demo.txt
    register: file_info

  - name: Print file existence
    debug:
      msg: "File exists!"
    when: file_info.stat.exists

  - name: Print file does not exist
    debug:
      msg: "File does not exist!"
    when: not file_info.stat.exists

* **lineinfile**

  * Add/modify a single line

  ```yaml
  - name: Modify line
    lineinfile:
      path: /etc/ssh/sshd_config
      regexp: '^PermitRootLogin'
      line: 'PermitRootLogin no'

* **blockinfile**

  * It adds, updates, or removes a block of text in a file, and keeps it managed using markers.

  ```yaml
  - name: Add config block
    blockinfile:
      path: /etc/myapp.conf
      block: |
        app.env=prod
        app.debug=false

* **replace**

  * Replace text using regex

  ```yaml
  - replace:
    path: /etc/httpd/conf/httpd.conf
    regexp: 'Listen 80'
    replace: 'Listen 8080'

* **fetch**

  * fetch copies files from the remote hosts to the Ansible controller (local machine).

  ```yaml
  - name: Download app.log from remote hosts
    fetch:
      src: /var/log/app/app.log
      dest: /tmp/ansible_logs/

### Package Management

* **yum / dnf (RHEL/Amazon Linux)**

  ```yaml
  - yum:
    name: httpd
    state: present

* **apt (Ubuntu/Debian)**

  ```yaml
  - apt:
    name: nginx
    state: present
    update_cache: yes

* **package (OS-agnostic wrapper)**

  ```yaml
  - package:
    name: git
    state: present

* **pip (Python packages)**

  ```yaml
  - name: Install Python flask package
    pip:
    name: flask
  ```
  ```yaml
  - name: Install specific version of Django
    pip:
    name: django==4.2


### Service Management

* **service**

  * Start/stop/restart services

  ```yaml
  - service:
    name: httpd
    state: started
    enabled: yes

* **systemd**

  * Preferred on modern systems

  ```yaml
  - systemd:
    name: nginx
    state: restarted
    daemon_reload: yes

### User & Group Management

* **user**

  * Create/delete users

  ```yaml
  - user:
    name: devops
    shell: /bin/bash
    state: present

* **group**

  ```yaml
  - group:
    name: developers
    state: present

* **authorized_key**

  * SSH key management

  ```yaml
  - authorized_key:
    user: devops
    key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

### Command Execution

* **command**

  * No shell features

  ```yaml
  - command: uptime

* **shell**

  * With pipes, redirects, variables

  ```yaml
  - shell: df -h | grep xvda

### Facts, Variables & Debugging

* **setup**

  * Gather system facts

  ```yaml
  - name: Gather facts from remote hosts
    setup:
  - name: Show hostname
    debug:
      var: ansible_hostname

* **debug**

  * Print variables

  ```yaml
  - debug:
    msg: "Host IP is {{ ansible_default_ipv4.address }}"

* **set_fact**

  * create or update variables during a playbook run.

  ```yaml
  - name: Set a custom fact
    set_fact:
    my_variable: "Hello, Ansible!"
  - name: Use the custom fact
    debug:
    msg: "The value of my_variable is {{ my_variable }}"

### Task Control & Conditionals

* **loop / with_items**

  * loop and with_items are used to run the same task multiple times with different items.

  * **loop**

  ```yaml
  - name: Install packages
    ansible.builtin.yum:
      name: "{{ item }}"
      state: present
    loop:
      - httpd
      - git
      - vim
  ```
  * **with_items**

  ```yaml
  - name: Install packages
    ansible.builtin.yum:
      name: "{{ item }}"
      state: present
    with_items:
      - httpd
      - git
      - vim

* **when**

  * Conditional execution

  ```yaml
  - name: Only run on Ubuntu
    ansible.builtin.debug:
      msg: "This host is Ubuntu"
    when: ansible_facts['os_family'] == "Debian"

* **register**

  * store the output of a task into a variable, which you can then use in subsequent tasks.'

  ```yaml
  - name: Run a command and register the output
    ansible.builtin.command: "hostname"
    register: hostname_result

  - name: Show the registered result
    ansible.builtin.debug:
      var: hostname_result

* **changed_when, failed_when**

  * control task behavior based on conditions:

    * changed_when → Force Ansible to consider a task changed or not changed.

    * failed_when → Force a task to fail based on a condition.
  
  * **changed_when**

  ```yaml
  - name: Check if file exists
    ansible.builtin.stat:
      path: /tmp/demo_file.txt
    register: file_status

  - name: Create file only if it does not exist
    ansible.builtin.command: "touch /tmp/demo_file.txt"
    changed_when: false   # Never show this task as changed
    when: file_status.stat.exists == false
  ```

  * **failed_when**
   
  ```yaml
  - name: Check content of a file
    ansible.builtin.command: "cat /tmp/demo_file.txt"
    register: file_content
    failed_when: "'ERROR' in file_content.stdout"

  - name: Show file content
    ansible.builtin.debug:
      var: file_content.stdout


### Git, CI/CD & Artifacts

* **git**

  * Clone a Git repository to a remote host.

  ```yaml
  - name: Clone a Git repository
  git:
    repo: https://github.com/ansible/ansible-examples.git
    dest: /tmp/ansible-examples
    version: main
    force: yes

* **get_url**

  * Download files

  ```yaml
  - get_url:
    url: https://example.com/app.tar.gz
    dest: /tmp/app.tar.gz

* **unarchive**

  * Extract tar/zip

  ```yaml
  - unarchive:
    src: /tmp/app.tar.gz
    dest: /opt/
    remote_src: yes

