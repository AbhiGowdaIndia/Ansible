### Installation on Ubuntu / Debian

**sudo apt update**

**sudo apt install -y ansible**

* Verify: 
**ansible --version**

### Installation on RHEL / CentOS / Rocky

**sudo yum install -y epel-release**

**sudo yum install -y ansible**

* Verify: 
**ansible --version**

### Installation on macOS

**brew install ansible**
 
* Verify: 
**ansible --version**

### Install via PIP

* Install Python & pip
  
  **sudo apt install -y python3 python3-pip**

* Install Ansible

  **pip3 install --user ansible**

* Add to PATH (if needed):

  **export PATH=$PATH:$HOME/.local/bin**

* Verify:
**ansible --version**

#### Post-Installation Checklist (IMPORTANT)

ansible --version
ansible-galaxy --version
ansible-config dump | head