# Ansible Modules

### The module which we commonly used in ansible playbook / roles

### File & Directory Management

* **copy**

  * Copy files from control node → managed nodes

  ```yaml
  - copy:
    src: my.conf
    dest: /etc/my.conf
    owner: root
    mode: '0644'