# guide-to-install-configure-Ansible-Server-host-configuration

### Summary

- **Installation**: Install Ansible on your control node.
- **SSH Key-Based Authentication**: Set up passwordless SSH between your control node and managed hosts.
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

### Step 3: Configure Ansible Inventory

The inventory file (`hosts.ini`) defines the managed hosts that Ansible will control. Create an inventory file and add your managed hosts:

```bash
sudo nano /etc/ansible/hosts
```

Add the following lines to the file:

```ini
[webservers]
server1 ansible_host=192.168.1.10
server2 ansible_host=192.168.1.11
```

Replace `server1`, `server2`, `192.168.1.10`, and `192.168.1.11` with your actual server hostnames or IP addresses.

### Step 4: Configure Ansible Configuration File (Optional)

The Ansible configuration file (`ansible.cfg`) allows you to configure various settings. You can leave this file as default or customize it according to your needs. By default, this file is not mandatory for running Ansible:

```bash
sudo nano /etc/ansible/ansible.cfg
```

Here are some common settings that you might want to configure:

```ini
[defaults]
inventory = /etc/ansible/hosts
remote_user = your_remote_user
private_key_file = /path/to/your/private/key
```

Replace `your_remote_user` and `/path/to/your/private/key` with your actual remote user and private key file path.

### Step 5: Testing Ansible

Now that Ansible is installed and configured, you can test it by running an ad-hoc command against your managed hosts. For example, to check connectivity:

```bash
ansible all -m ping
```

This should return successful pings from your managed hosts.


