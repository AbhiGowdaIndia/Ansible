# Roles

* An Ansible Role is a structured way to organize playbooks.

* It splits your automation into separate, reusable components.

* A role contains:

  * Tasks

  * Variables

  * Files

  * Templates

  * Handlers

  * Defaults

  * Meta info

* Roles make playbooks clean, reusable, readable, and scalable.

### 📁 Standard Role Folder Structure
```yaml
myrole/  
|  
├── defaults  
|   |  
│   └── main.yml  
|  
├── vars  
|   |  
│   └── main.yml  
├── tasks  
|   |  
│   └── main.yml  
|  
├── handlers  
|   |  
│   └── main.yml  
|  
├── files  
|   |  
│   └── (static files go here)  
|  
├── templates  
|   |  
│   └── (Jinja2 templates go here)  
|  
├── meta  
|   |  
│   └── main.yml  
|  
└── README.md  
```
#### Purpose of Each Directory

* **defaults/main.yml**

  * Lowest priority variables

  * Good for variable values that users may override

  * Example:

    ```yaml
    http_port: 80
    ```

* **vars/main.yml**

  * Higher priority variables

  * Usually for variables not meant to be overridden

  * Example:

    ```yaml
    service_name: httpd
    ```

* **tasks/main.yml**

  * Contains the main logic of your role

  * Calls other YAML files if needed

  * Example:

    ```yaml
    - name: Install Apache package
      yum:
        name: httpd
        state: present
    ```

* **handlers/main.yml**

  * Contains handlers triggered by notify

  * Used for restarting services

  * Example:

    ```yaml
    - name: restart apache
      service:
        name: httpd
        state: restarted
    ```

* **files/**

  * Contains static files (no Jinja)

  * Used with copy: or file: module

  * Example:

    ```yaml
    files/
      └── index.html
    ```

* **task/**

  * Tasks are the smallest unit of action in Ansible and are organized in the tasks/ directory of a role. 

  * Example:
    
    ```yaml
    - name: Copy html file
      copy:
        src: index.html
        dest: /var/www/html/index.html
    ```

* **templates/**

  * Contains Jinja2 templates (*.j2)

  * Example:

    ```yaml
    templates/
        └── httpd.conf.j2
    ```

* **meta/main.yml**

  * Includes role dependencies

  * Example:

    ```yaml
    dependencies:
      - common_role
    ```
