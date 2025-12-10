* **serial** controls batch size

* how many hosts Ansible runs the playbook on at one time.

* Instead of running on all hosts simultaneously, Ansible runs the playbook in chunks (batches).

**Example 1:**

```yaml
---
- name: Rolling update of web servers
  hosts: web
  serial: 2
  become: yes

  tasks:
    - name: Restart web service
      service:
        name: httpd
        state: restarted
```
* In the above example asume we have 6 hosts in the group "web", ansible would execute the play completely on 2 of the hosts before moving on to the next 2 hosts and so on.

**Exampl 2:**

```yaml
- name: testplay
  hosts: web
  serial: "30%"
  become: yes
  tasks:
    - name: Restart web service
      service:
        name: httpd
        state: restarted
```
* We can also specify the percentage with the serial keyword. Ansible applys the percentage to the total number of hosts in a play to determine the number of hosts per pass.

* If number of hosts does not devide equally into the number of passes, the final pass contains the reminders.

**Example 3:**

```yaml
- name: testplay
  hosts: web
  serial:
    - 1
    - 3
    - 5
  become: yes
  tasks:
    - name: Restart web service
      service:
        name: httpd
        state: restarted
```

* We can also specify batch as a list

* In the above example, first batch would contain 1 host, the next would contain 3 hosts, (if there any hosts left) every following batch would contain either 5 hosts or all the remaining hosts if fewer than 5.

**Example 4:**

```yaml
- name: testplay
  hosts: web
  serial:
    - "10%"
    - "30%"
    - "50%"
  become: yes
  tasks:
    - name: Restart web service
      service:
        name: httpd
        state: restarted
```

* We can also list multiple batch sizes as percentage.

**Example 5:**

```yaml
- name: testplay
  hosts: web
  serial:
    - "1"
    - "3"
    - "20%"
  become: yes
  tasks:
    - name: Restart web service
      service:
        name: httpd
        state: restarted
```

* We can also mix and match the values.