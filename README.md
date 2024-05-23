# guide-to-install-configure-Ansible-Server-host-configuration

### Summary

- **Installation**: Install Ansible on your control node.
- **SSH Key-Based Authentication**: Set up passwordless/withPassword SSH between your control node and managed hosts.
- **Inventory**: Configure the Ansible inventory file to list your managed hosts.
- **Configuration File (Optional)**: Customize the Ansible configuration file according to your preferences.
- **Testing**: Test Ansible by running an ad-hoc command to verify connectivity.


This guide provides a basic setup for an Ansible control node. Customize further based on your environment and specific requirements.
Setting up an Ansible control node involves installing Ansible and configuring it to manage other hosts. 
Here’s a detailed step-by-step guide to install and configure an Ansible control node on a Linux host.

### Prerequisites

1. **Control Node Requirements**:
   - A Linux host (Ubuntu, Debian, CentOS, RHEL, etc.) that will act as your Ansible control node.
   - Internet access to download Ansible and its dependencies.

### Step 1: Install Ansible on the Control Node

#### For Ubuntu/Debian:

1. Update the package index:

   ```bash
   sudo apt update
   ```

2. Install Ansible:

   ```bash
   sudo apt install ansible
   ```

#### For CentOS/RHEL:

1. Enable the EPEL repository (if not already enabled):

   ```bash
   sudo yum install epel-release
   ```

2. Install Ansible:

   ```bash
   sudo yum install ansible
   ```

#### Verify Installation:

After installation, verify that Ansible is installed correctly:

```bash
ansible --version
```

This should display the installed version of Ansible.

### Step 2: Configure SSH Key-Based Authentication

Ansible manages remote hosts using SSH. To enable passwordless SSH access from the control node to managed hosts, set up SSH key-based authentication:

1. Generate an SSH key pair (if you haven't already):

   ```bash
   ssh-keygen
   ```

   Follow the prompts to generate the SSH key pair. You can press Enter to accept the default location (`~/.ssh/id_rsa`) and no passphrase if you want to enable passwordless SSH.

2. Copy the SSH public key to your managed hosts:

   ```bash
   ssh-copy-id username@hostname_or_IP
   ```

   Replace `username` with your username on the managed host and `hostname_or_IP` with the hostname or IP address of your managed host.

   Example:

   ```bash
   ssh-copy-id user@192.168.1.10
   ```

   You will be prompted to enter the password for the user on the managed host.

3. Test SSH connection:

   ```bash
   ssh username@hostname_or_IP
   ```

   Ensure that you can SSH to the managed host without entering a password.

### Step 3 : Configure Ansible Inventory

The inventory file is where you define your managed hosts. Create an inventory file (e.g., `hosts.ini`) and add the IP addresses or hostnames of your managed hosts:

```ini
[web_servers]
server1 ansible_host=192.168.1.10
server2 ansible_host=192.168.1.11
```

Save this file in a directory of your choice. For example, you could create a directory named `inventory` and save the `hosts.ini` file inside it.

### Step 4: Install SSH Server

If the SSH server is not already installed on your Ansible control node, you can install it using the following command:

```bash
sudo apt install openssh-server
```

### Step 5: Configure SSH Password Authentication

By default, SSH password authentication should be enabled. If it's not, you can enable it by editing the SSH configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Ensure the following configuration options are set:

```bash
PasswordAuthentication yes
```

Save the file and then restart the SSH service:

```bash
sudo systemctl restart ssh
```

### Step 6: SSH Password Authentication for Managed Hosts

Ensure that password authentication is enabled on the managed hosts as well. Edit the SSH configuration file on each managed host:

```bash
sudo nano /etc/ssh/sshd_config
```

Ensure the following configuration options are set:

```bash
PasswordAuthentication yes
```

Save the file and then restart the SSH service on each managed host:

```bash
sudo systemctl restart ssh
```

### Step 7: Test SSH Connection

Test the SSH connection to ensure that Ansible can communicate with the managed hosts using password authentication:

```bash
ansible all -m ping -i /path/to/your/inventory/hosts.ini -u username -k
```

Replace `/path/to/your/inventory/hosts.ini` with the actual path to your inventory file, `username` with your username on the managed hosts, and `-k` prompts for the SSH password.

### Step 8: Ansible Configuration File (Optional)

Ansible uses a configuration file (`ansible.cfg`) to control its behavior. While most settings have sensible defaults, you may want to customize some settings based on your requirements.

Create an `ansible.cfg` file in the same directory as your inventory file or in the `/etc/ansible/` directory:

```ini
[defaults]
inventory = /path/to/your/inventory/hosts.ini
remote_user = username
ask_pass = True
```

Replace `/path/to/your/inventory/hosts.ini` with the actual path to your inventory file and `username` with your username on the managed hosts.

### Step 9: Verify Configuration

Test your Ansible setup by running a simple command against all hosts in your inventory:

```bash
ansible all -m command -a "hostname" -i /path/to/your/inventory/hosts.ini -u username -k
```


You have now installed and configured an Ansible control node on Ubuntu, using passwordless or password-based authentication for SSH. You can use this control node to manage multiple hosts efficiently using Ansible playbooks and modules. Make sure to explore more advanced Ansible features and practices to streamline your infrastructure management tasks.

