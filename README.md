# Ansible-Notes
Ansible Basics to Advance Commands with Installation

* Installation on AWS Ubuntu Instances
   1 Node as Master and 1 Node as a Worker 

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/10877c25-76e8-4470-b5de-ab22ec4bfb44)

Note: Installation steps will performed only on Master node.

**Worker Node Steps**

      sudo su

      hostnamectl set-hostname master
      
      apt update -y

      apt install software-properties-common -y

      add-apt-repository --yes --update ppa:ansible/ansible

      apt install ansible -y

      ansible --version

      vim /etc/hosts  

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/d5c9d497-db37-42f9-83a4-7c81370da64b)

      vim /etc/ansible/hosts

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/66bd3d59-2643-4412-8a31-eda09b8d81bf)

**Worker Node Steps**

* Login to Worker node for Password-less Login

        vim /etc/ssh/sshd_config

* PermitRootLogin yes / PasswordAuthentication yes

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/031cc3ac-1606-4f04-a286-10c065c143c9)

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/da90c5ef-f94f-47a2-b41c-566e17039d0d)

      systemctl restart sshd

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/a33f0754-c014-47e3-be48-5e2d2f23155e)

**Master Node Steps**

      ssh-keygen

      ssh-copy-id root@node1

      ansible test -m ping

  # Deploy Ansible >> Defining Inventory

* Inventory: Information about targeted managed nodes stored in a file called inventory.

* Types of Inventory
  1. Static Inventory
  2. Dynamic Inventory
 
  Static Inventory
  1. INI Format > Simple Text Format (Initialization)
  2. YAML Format > Human Readable Format
 
  Dynamic Inventory
  1. Python Script

      vim /etc/ansible/hosts

     [test]

      node1

      [linux]    # multiple host names should be defined like this

      servera

      server[b:d]

      [storage]   # multiple Ip addresses should be defined like this

      192.168.0.1

      192.168.[0:3].2

      [nfs]      # multiple Ip addresses should be defined like this

      172.125.[1:4].[2:5]

      [samba:children] # Group of Groups should be defined as children

      storage

      nfs

      ansible test --list-hosts   [show list of hosts]

      ansible linux --list-hosts

      ansible all --list-hosts

      ansible ungrouped --list-hosts

  Note: Above mentioned is default inventory location, We can create our own inventory, name can be anything, but the method to execute is different.

  -i is used to used our custom inventory, but we must present at the location where inventory reside.

      mkdir project1

* cat myinventory 

[web]

node1

         ansible web --list-hosts -i myinventory

# Managing Ansible Configuration Files

# Priorities of ansible configuration files

Four Priorites of ansible configuration files

1. ANSIBLE_CONFIG => Enviroment varible

2. ./ansible.cfg  => Current Directory

3. ~/. ansible.cfg => Home directory hidden file

4. /etc/ansible/ansible.cfg   => Default & last


         ansible --version

  **config file = /etc/ansible/ansible.cfg**

  ====================================================================

Enviroment varible

      echo $ANSIBLE_CONFIG
      
      vim .bashrc

      export ANSIBLE_CONFIG=/tmp/ansible.cfg
      
      touch /tmp/ansible.cfg
      
      source .bashrc
      
      echo $ANSIBLE_CONFIG
      
      ansilbe --version

![image](https://github.com/sunnyvalechha/Ansible-on-Ubuntu/assets/59471885/73196982-f756-4699-8be8-d26d4b035f6e)

====================================================================

Current Directory (Must present at location)

      cd /var

      touch ansible.cfg

      ansible --version

![image](https://github.com/sunnyvalechha/Ansible-on-Ubuntu/assets/59471885/ddb019c4-93fa-4d67-afaa-5d4c90536be9)

====================================================================

Home Directory Hidden File

      touch .ansible.cfg

      ansible --version

![image](https://github.com/sunnyvalechha/Ansible-on-Ubuntu/assets/59471885/46171c01-b8f0-4581-a56f-01f675f7dae2)

      rm -rf .ansible.cfg
====================================================================

Note: If above 3 priorities are not present, it will consider the last one as default priority

![image](https://github.com/sunnyvalechha/Ansible-on-Ubuntu/assets/59471885/667df24d-3bff-4da1-81e4-50cb5aa17ed8)

====================================================================

# Running Ad-hoc commands 

Ad-hoc commands are quick commands running on terminal used when we do not need to save for future use. 

Attributes of ad-hoc commands

-m > module

-a > arguments 

-i > inventory

      ansilble web -m ping

      ansible test -m shell -a 'id'

      ansible test -m shell -a 'id user1'

      ansible test -m user -a 'name=john state=present'

      ansible test -m shell -a "echo '123' | passwd --stdin john"

      ansible test -m user -a 'name=john state=absent'

      ansible test -m group -a 'name=team_dev state=present'

      ansible test -m shell -a 'cat /etc/group | grep team'

      ansible test -m shell -a 'hostname'

      ansible test -m shell -a 'uptime'

      ansible test -m shell -a 'df -h'

      ansible test -m copy -a 'src=/root/file.txt dest=/root'

      ansible test -m file -a 'dest=/root/myd1 state=directory'

      ansible test -m file -a 'dest=/root/myf1 state=touch'

      ansible test -m file -a 'dest=/root/myd1 state=absent'

      ansible test -m file -a 'dest=/root/myd2 state=directory mode=754'

      ansible test -m file -a 'dest=/root/myd2 state=directory mode=754 owner=user1 group=student'

      ansible test -m lineinfile -a 'dest=/root/file.txt line="This is test line"'

      ansible test -m synchronize -a 'src=/root/file2.txt dest=/root'

      ansible test -m fetch -a 'src=/root/bottle dest=/root'

      ansible test -m service -a 'name=sshd state=restarted enabled=true'

      ansible test -m firewalld -a "service=http state=enabled immediate=yes permanent=yes"

      ansible test -m firewalld -a "service=http state=disabled"


**Access man pages / documentation about modules**

      ansible-doc user

Note: If file is not opening in vim but in nano, change the default editor option.

      dpkg -l | grep vim

      which vim

      update-alternatives --install /usr/bin/editor editor /usr/bin/vim 100

**Managing settings in the configuration files**

Sections of configuration files

      grep ^[[] /etc/ansible/ansible.cfg

For Basic Operation we use only 2 sections

1. [defaults]
   
2. [privilege_escalation]

[defaults]

inventory=./hosts_file

remote_user=anyuser

ask_pass=true

[privilege_escalation]

become=true  **#(If false, user cannot perform root user tasks, regular user tasks can be done, can also be used --become )**

become_method=sudo **#(Default in Linux)**

become_user=root **#(Always root/superuser)**

become_ask_pass=true **#(will not ask sudo password on managed hosts, or make entry in visudo file as no password required -K )** 

**Run all ansible commands from a regular user**

Few Important points to remember:

1. A regular user whom we are running commands from master, a user with same name must present at the worker node also becuase user connect with user with same name.

2. User must have set password on worker node.

3. User must added in /etc/sudoers file on worker node (requires root privileges).

4. If you want to test what password have set to the user on worker node run this command "ssh -o PreferredAuthentications=password sunny@node1" this will not logged as ssh keys, It will logged as password.

5. -k and -K > same password will be used

6. If you don't generate passwrod ssh keys in that case -k will work

7. If we don't mention inventory file in user's hosts file. It will gather information from default location /etc/ansible/hosts

**Use privileges from Ad-hoc commands**

-K > sudo password (become_ask_pass)

-k > prompt for ssh password (ask_pass)

-u > remote user name

-i > point to inventory file

         ansible test -m shell -a "tail -n 1 /etc/shadow" --become --become-user=root --become-method=sudo -K -k





         
