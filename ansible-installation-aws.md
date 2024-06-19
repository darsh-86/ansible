# Installation process of Ansible Server

# Set hostname
'''bash
hostnamectl set-hostname control
'''

# Install EPEL repository
'''bash
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install epel-release-latest-7.noarch.rpm -y
'''

# Update system packages
'''bash
yum update -y
'''

# Install necessary packages
'''bash
yum install git python python-level python-pip openssl ansible -y
'''

# Check Ansible installation
'''bash
ansible --version
'''

# Configure Ansible hosts file
'''bash
vi /etc/ansible/hosts
# Add the following under '[demo]' group:
# private_ip_of_node-1
# private_ip_of_node-2
'''

# Configure Ansible global configuration
'''bash
vi /etc/ansible/ansible.cfg
# Uncomment and modify:
# inventory = /etc/ansible/hosts
# sudo_user = root
'''

# Create 'ansible' user on all instances
'''bash
adduser ansible
passwd ansible
'''

# Grant sudo permissions to 'ansible' user
'''bash
visudo
# Add: ansible ALL=(ALL) NOPASSWD: ALL at line 101
'''

# SSH configuration on all nodes
'''bash
vi /etc/ssh/sshd_config
# Uncomment:
# PermitRootLogin yes (line 38)
# PasswordAuthentication yes (line 61)
# Comment:
# #PasswordAuthentication no (line 63)
service sshd restart
'''

# Switch to 'ansible' user on all nodes
'''bash
su - ansible
ssh <private_ip_of_node> # Access each node
'''

# Generate SSH keys on Ansible server
'''bash
ssh-keygen
cd .ssh
ssh-copy-id ansible@<private_ip_of_node>
# Enter ansible user's password
'''

# Verify hosts and groups
'''bash
ansible all --list-hosts
ansible all --list-hosts
'''

# Ad-hoc Commands
'''bash
ansible demo -a "ls"
ansible all -a "ls"
'''

'''bash
ansible demo -a "sudo yum install httpd -y"
'''

'''bash
ansible demo -ba "yum remove httpd -y" # Use 'ba' to avoid sudo prompt
'''