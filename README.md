# Ansible

## This repository covers all the necessary commands you need when you use Ansible.

#### What is Ansible?
Ansible is an open-source automation tool developed by Red Hat, designed to automate tasks such as configuration management, application deployment, and IT orchestration. It is known for its simplicity, ease of use, and powerful capabilities that allow users to manage large-scale IT environments efficiently. Ansible uses a simple, human-readable language (YAML) to describe automation jobs, making it accessible even to those who are not experienced programmers.
## Installation
### Steps to install Ansible:
#### On Ubuntu:
  ##### Update your package index
    sudo apt update
  ##### Install Ansible
    sudo apt install ansible -y
  ##### Verify the installation
    ansible --version
#### On CentOS:
  ##### Enable EPEL repository
    sudo yum install epel-release -y
  ##### Install Ansible
    sudo yum install ansible -y 
  ##### Verify the installation
    ansible --version
## Configuration
### Setting up the Ansible inventory:
#### Create an inventory file:
      sudo nano /etc/ansible/hosts
##### Example of inventory file content:
        [webservers]
        web1.example.com
        web2.example.com
        
        [dbservers]
        db1.example.com
        db2.example.com
#### Configuring Ansible:
##### Edit the Ansible configuration file:
        sudo nano /etc/ansible/ansible.cfg
#### Basic configuration example:
        inventory = /etc/ansible/hosts
        remote_user = your_username
        private_key_file = ~/.ssh/id_rsa
        host_key_checking = False
#### Basic Commands
##### 1. Ping all hosts to check connectivity:
        ansible all -m ping
##### 2. Run a command on all web servers:
        ansible webservers -m command -a "uptime"
##### 3. Gather facts about all hosts:
        ansible all -m setup
##### 4. Copy a file to all hosts:
        ansible all -m copy -a "src=/path/to/local/file dest=/path/to/remote/file"
##### 5. Install a package on all hosts:
        ansible all -m apt -a "name=package_name state=present" -b
### Ad-hoc Commands
#### Run a Command on All Hosts:
        ansible all -a "command"
#### Run a Command with sudo:
        ansible all -a "command" -b
#### Copy a File to All Hosts:
        ansible all -m copy -a "src=/path/to/local/file dest=/path/to/remote/file"
#### Gather Facts about All Hosts:
        ansible all -m setup

## Playbooks
### Writing and running a simple playbook:
#### Example playbook to update and upgrade packages:
        ---
        - name: Update and upgrade packages
          hosts: all
          become: yes
          tasks:
            - name: Update apt cache
              apt:
                update_cache: yes
              register: update_cache
        
            - name: Upgrade all packages
              apt:
                upgrade: dist
              when: update_cache.changed
#### Running the playbook:
            ansible-playbook update_upgrade.yml
#### Run a Playbook with Specific Inventory:
           ansible-playbook -i inventory_file playbook.yml
#### Check Playbook Syntax:
           ansible-playbook playbook.yml --syntax-check
#### Dry Run a Playbook:
          ansible-playbook playbook.yml --check

## Roles
### Creating and using roles:
#### Creating a role:
        ansible-galaxy init role_name
#### Install Roles from Galaxy:
        ansible-galaxy install role_name

#### Directory structure of a role:
        myrole/
        ├── README.md
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
#### Example task in a role (tasks/main.yml):
      ---
      - name: Ensure Nginx is installed
        apt:
          name: nginx
          state: present
      
      - name: Start Nginx service
        service:
          name: nginx
          state: started
          enabled: yes
#### Using the role in a playbook:
        ---
        - name: Apply MyRole
          hosts: webservers
          roles:
            - myrole
## Advanced Topics
### Ansible Vault:
#### Creating a new encrypted file:
      ansible-vault create secret.yml
#### Editing an encrypted file:
      ansible-vault edit secret.yml
#### Encrypting an existing file:
      ansible-vault encrypt existing_file.yml
#### Decrypting a file:
      ansible-vault decrypt secret.yml
### Using Ansible Galaxy:
#### Installing a role from Ansible Galaxy:
      ansible-galaxy install username.rolename
#### Listing installed roles:
      ansible-galaxy list
### Example of an Ansible Galaxy role (requirements.yml):
      ---
      roles:
        - name: geerlingguy.apache
          version: 3.0.0
### Installing roles from requirements file:
      ansible-galaxy install -r requirements.yml
## Utility Commands
#### Generate SSH Keys:
      ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
#### Copy SSH Key to Remote Host:
      ssh-copy-id user@remote_host
## Miscellaneous Commands
#### Display Ansible Configuration:
      ansible-config dump
#### Run Ad-hoc Command with Specific Inventory:
      ansible -i inventory_file all -m ping
#### Run Command with Specific User:
      ansible all -a "command" -u username
