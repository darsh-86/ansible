# Installation Process of Ansible Server

### Step 1: Set Hostname
```sh
hostnamectl set-hostname control
```

### Step 2: Install EPEL Repository
```sh
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install epel-release-latest-7.noarch.rpm -y
```

### Step 3: Update System Packages
```sh
yum update -y
```

### Step 4: Install Necessary Packages
```sh
yum install git python python-level python-pip openssl ansible -y
```

### Step 5: Check Ansible Installation
```sh
ansible --version
```

### Step 6: Configure Ansible Hosts File
```sh
vim /etc/ansible/hosts
```
Add the following under the `[demo]` group:
```ini
[demo]
private_ip_of_node-1
private_ip_of_node-2
```

### Step 7: Configure Ansible Global Configuration
```sh
vi /etc/ansible/ansible.cfg
```
Uncomment and modify:

- `inventory = /etc/ansible/hosts`
- `sudo_user = root`

### Step 8: Create 'ansible' User on All Instances
```sh
adduser ansible
passwd ansible
```

### Step 9: Grant Sudo Permissions to 'ansible' User
```sh
visudo
```
Add the following line at the end of the file:
```ini
ansible ALL=(ALL) NOPASSWD: ALL
```

### Step 10: SSH Configuration on All Nodes
```sh
vi /etc/ssh/sshd_config
```
Uncomment the following lines:

- `PermitRootLogin yes` (line 38)
- `PasswordAuthentication yes` (line 61)
- `PasswordAuthentication no` (line 63)

```sh
service sshd restart
```

### Step 11: Switch to 'ansible' User on All Nodes
```sh
su - ansible
```
```sh
ssh <private_ip_of_node> # Access each node
```

### Step 12: Generate SSH Keys on Ansible Server
```sh
ssh-keygen
cd .ssh
```
```sh
ssh-copy-id ansible@<private_ip_of_node>
```
Enter the password for the `ansible` user.

### Step 13: Verify Hosts and Groups
```sh
ansible all --list-hosts
```

### Step 14: Ad-hoc Commands
```sh
ansible demo -a "ls"
```

```sh
ansible all -a "ls"
```

```sh
ansible demo -a "sudo yum install httpd -y"
```

```sh
ansible demo -ba "yum remove httpd -y" # Use 'ba' to avoid sudo prompt
```

### Additional Tips

- Ensure that the `ansible` user has the necessary permissions to access the nodes.
- Verify that the SSH keys are correctly generated and copied to the nodes.
- Use `ansible-playbook` for more complex automation tasks.

### Troubleshooting

- Check the Ansible logs for any errors or issues.
- Verify that the nodes are correctly configured and accessible.
- Use `ansible --version` to ensure that Ansible is correctly installed and configured.
