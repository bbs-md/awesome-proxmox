# Postfix setup on VMs, Proxmox nodes and Proxmox Backup Server via Ansible playbook
  
  These Ansible playbook configures the postfix service on VMs.

## Prerequisites
  ### 1. Install `Ansible` on a certain Linux host from which Ansible playbooks are to be run, this host is called for brevity below `Ansible host`

  For `Debian` Linux

  ```
  sudo apt update
  sudo apt install ansible
  ```

  or follow [step 2](https://github.com/Alliedium/awesome-ansible#setting-up-config-machine) of `Setting up config machine` section to install the latest Ansible version

  ### 2. Clone this repo

  ```
  git clone https://github.com/Alliedium/awesome-proxmox.git $HOME/awesome-proxmox
  ```

  ### 3. Install Ansible postfix role - [Oefenweb/ansible-postfix](https://github.com/Oefenweb/ansible-postfix)

  ```
  ansible-galaxy install -r $HOME/awesome-proxmox/postfix/playbooks/config-postfix/requirements.yml
  ```

  ![install_postfix_role](./images/install_postfix_role.png)

  Check installed role

  ```
  ansible-galaxy list
  ```

  
## **Ansible-vault**
 
  Ansible vault provides a way to encrypt and manage sensitive data such as passwords. We use Ansible vault to store `pve_postfix_sasl_user` and `pve_postfix_sasl_password` variables

  ```
  cp $HOME/awesome-proxmox/postfix/playbooks/config-postfix/secrets-file.enc.example $HOME/awesome-proxmox/postfix/playbooks/config-postfix/secrets-file.enc
  ```
  
  ![secrets](./images/secrets-file.png)

  Encrypt `$HOME/awesome-proxmox/postfix/playbooks/config-postfix/secrets-file.enc`  file and set a password by command

  ```
  ansible-vault encrypt $HOME/awesome-proxmox/postfix/playbooks/config-postfix/secrets-file.enc
  ```

  ![encrypt file](./images/secrets-file_1.png)

  `$HOME/awesome-proxmox/postfix/playbooks/config-postfix/secrets-file.enc`  file is encrypted

  ![encrypted_file file](./images/encrypted_file.png)

## **Ansible postfix playbook**

  ```
  cp $HOME/awesome-proxmox/postfix/inventory $HOME/awesome-proxmox/postfix/my-inventory
  ```

  - In `$HOME/awesome-proxmox/postfix/my-inventory/hosts.yml` file replace hosts IP addresses to your hosts IP addresses and edit the path to private key to match it on your machine. Our hosts are in fact include our Proxmox nodes as well as (possibly) Proxmox Backup Server

 
  **Run ansible playbook**
 
  Run the script

  ```
  $HOME/awesome-proxmox/postfix/playbooks/config-postfix/run.sh
  ```

  You could provide your `inventory` folder (path to your inventory folder) as script parameter

  ```
  $HOME/awesome-proxmox/postfix/playbooks/config-postfix/run.sh [<inventory_path>]
  ```

  If the path given by `<inventory_path>` is not given, then `../../my-inventory/` will be used by default (according to the name of the inventory above).