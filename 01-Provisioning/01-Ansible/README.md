# 01-Homelab/01-Provisioning/01-Ansible
This section describes how to provision a Kubernetes cluster using *Ansible*.

## Step-by-step approach

### 1. Prerequisites
Before you can start you have to prepare the stage.

#### 1.1 Install Virtualbox
Please install Virtualbox on your box.

Here you can find an install script for Ubuntu: https://github.com/gerundium/devops_tools/blob/main/Virtualbox/dbc-install-virtualbox.sh

#### 1.2 Install Ansible
I recommend to install Ansible as python package in a virtual environment. Following this approach allows you to pin Ansible to a specific version and allows multiple environments with different versions for testing purposes.

*Ubuntu example:*
   ```bash
   sudo apt update
   sudo apt install python3-pip python3-venv

   mkdir -p ~/.virtualenvs
   python -m venv ~/.virtualenvs/environment

   source ~/.virtualenvs/environment/bin/activate

   python3 -m pip -V

   python3 -m pip install ansible
   python3 -m pip install --upgrade ansible
   ```

*Install requirements*
   ```bash
   ansible-galaxy collection install -r collections/requirements.yml
   ```

#### 1.3 Prepare ISO template
Go to talos.dev and download the metal iso to "***$HOME/Downloads***"

For Ubuntu:

```bash
cd $HOME/Downloads

curl -OL https://github.com/siderolabs/talos/releases/download/v1.6.3/metal-amd64.iso
```

#### 1.4 Out of scope
If you stick to the example you will provision 3 vms (1 control and 2 worker nodes). Therefor you need 3 ip addresses, 3 names and 3 mac addresses.

1.2.1 Create a A-record for every node on your dns server (In a home lab probably your router)

   ```
   k8s-control-1.example.com = 192.168.1.100
   k8s-node-1.example.com = 192.168.1.101
   k8s-node-2.example.com = 192.168.1.102
   ```

1.2.2 Create static DHCP leases for the new mac addresses
Because mac addresses have to be uniqe in a network segment you can reuse the addresses from the example. But this depends heavily on your netwok setup. As long as they are uniqe you should not have any issues with them.

### 2 Configuration
Before you are able to deploy you have to set some base values.

#### 2.1 Inventory
Customize some variables to fit your setup.
1. Make a copy of this directory "**Ansible/inventory/example.dist**" (Example stands for the name of your cluster)
2. Apply your changes to
   1. Ansible/inventory/yourClusterName/hosts.ini
   2. Ansible/inventory/yourClusterName/host_vars/example # You have to rename example to the name of your Virtualbox box or ansible will not apply the variables on that box!

#### 2.2 Ansible
Customize the Ansible settings:

1. Make a copy of Ansible/ansible.cfg.dist (Ansible/ansible.cfg)
2. Customize the file. At least, you have to change the path for the "inventory" variable.

#### 2.3 Make
The make scripts are shortcuts for actions like ansible-playbook ...

If you want to use them (and you should).:

1. Copy Ansible/makefile.dist to Ansible/makefile
2. Custiomize the file

### 3. Build the virtual machines
One last step. Run a bash terminal and change into the directory where the makefile resides and just execute make.
```bash 
make
```