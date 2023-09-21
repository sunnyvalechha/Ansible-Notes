# Ansible-Notes
Ansible Basics to Advance Commands with Installation

* Installation on AWS Ubuntu Instances
   1 Node as Master and 1 Node as a Worker 

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/10877c25-76e8-4470-b5de-ab22ec4bfb44)

Note: Installation steps will performed only on Master node.

**Worker Node Steps**

* sudo su                 [ take root access }

* hostnamectl set-hostname master   [ set hostname for both machine ]

* apt update -y

* apt install software-properties-common -y

* add-apt-repository --yes --update ppa:ansible/ansible

* apt install ansible -y

* ansible --version    [ check version / validation of Ansible Installation ] 

* vim /etc/hosts  [ make host entry for worker node]

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/d5c9d497-db37-42f9-83a4-7c81370da64b)

* vim /etc/ansible/hosts [ make host entry for ansible hosts]

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/66bd3d59-2643-4412-8a31-eda09b8d81bf)

**Worker Node Steps**

* Login to Worker node for Password-less Login

* vim /etc/ssh/sshd_config

* PermitRootLogin yes / PasswordAuthentication yes

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/031cc3ac-1606-4f04-a286-10c065c143c9)

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/da90c5ef-f94f-47a2-b41c-566e17039d0d)

* systemctl restart sshd

![image](https://github.com/sunnyvalechha/Ansible-Notes/assets/59471885/a33f0754-c014-47e3-be48-5e2d2f23155e)

**Master Node Steps**

* ssh-keygen

* ssh-copy-id root@worker-1

* ansible test -m ping

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
 
* vim /etc/ansible/hosts
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

* ansible test --list-hosts   [show list of hosts]

* ansible linux --list-hosts


  
