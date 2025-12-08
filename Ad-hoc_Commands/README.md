# Ad-hoc Commands

* Ad-hoc commands are commands which can be run individually to perform quick task or function.

* Ad-hoc commands won't save for later.

* these are simple linux based commands, only a single command can be executed at a point of time.

### Commands

* Check connectivity  of all the hosts(very common)

  **ansible all -m ping**

* Check connectivity in a perticular group

  **ansible \<group-name> -m ping**

  Example:  
  **Note:** In all the example we use **Demo** as group  

    **ansible Demo -m ping**

* To reboot hots in a group

  **ansible Demo -a "/bin/reboot"**

* Check host system info 

  **ansible Demo -m setup | less**

* Transfer files to hosts in a group

  **ansible Demo -m copy -a "src=/home/ansible dest=/tmp/home"**

* create new user in all the hosts in a group

  * Basic ad-hoc command (add user)

    **ansible Demo -m user -a "name=devops state=present"**

  * Add user with a home directory

    **ansible Demo -m user -a "name=devops create_home=yes"**

  * Add user with a specific shell

    **ansible Demo -m user -a "name=devops shell=/bin/bash"**

  * Add user with sudo (very common)

    **ansible Demo -m user -a "name=devops groups=wheel append=yes" -b**  
    * -b → become root

  * Set password (hash required)

    * Generate password hash:

      **openssl passwd -6**

    * Use that hash:

      **ansible Demo -m user -a "name=devops password='$6$abc...xyz'"**

    * Create user with UID & comment

      **ansible Demo -m user -a "name=devops uid=1050 comment='DevOps User'"**

    * (all the option mention above - combined)

      **ansible Demo -m user -a "name=devops password='$6$abc...xyz' uid=1050 create_home=yes shell=/bin/bash groups=wheel append=yes" -b**

* Verify user creation

  **ansible Demo -m command -a "id devops"**

* Delete a user from hosts in a group

  **ansible Demo -m user -a "name=devops state=absent**

* Run a shell/command

  * This for simple commands.
  
    **ansible web -m command -a "ls -lrt"** 
  
  * For shell features like |, >, &&, use:

    **ansible web -m shell -a "ls -lrt | grep log"**

* Check if package(ex:httpd) is installed, if not install it in Hosts in a group

  **ansible Demo -m yum -a "name=httpd state=present"**

* Check if package(ex:httpd) is installed, if it is installed update it in Hosts in a group.

  **ansible Demo -m yum -a "name=httpd state=latest"**

*  To uninstall a package in hosts in a group

  **ansible Demo -m yum -a "name=httpd state=absent"**

* To start a service 

  **ansible Demo -m service -a "name=https state=started"**

* To stop a service

  **ansible Demo -m service -a "name=https state=stopped"**

* To restart a service

  **ansible Demo -m service -a "name=https state=restarted"**

* Check disk usage

  **ansible all -m command -a "df -h"**

* Check memory usage

  **ansible all -m shell -a "free -m"**

* Using **--limit** option to issue commands only on the specific host in the hosts file

  **ansible Demo -m ping --limit 172.31.33.230**