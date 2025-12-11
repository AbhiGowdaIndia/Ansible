# myrole

* This is an example for role.

### Create Role Manually (If You Don’t Want Galaxy Init)

* Just create folders yourself:

```yaml
  myrole/
  tasks/main.yml
  handlers/main.yml
  vars/main.yml
  defaults/main.yml
  templates/
  files/
```
* Then fill in the content as needed.

### Create a Role Using Ansible Galaxy 

* Run this command:
  
  **ansible-galaxy init myrole**

* This will create the following folder structure:

```yaml
myrole/
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```

#### Contents

* **tasks/main.yml →** main list of tasks

* **handlers/main.yml →** handlers for notify

* **defaults/main.yml →**  lowest precedence variables

* **vars/main.yml →** higher-precedence variables

* **files/ →** direct copy files

* **templates/ →** Jinja2 templates

* **meta/main.yml →** role metadata (dependencies)


