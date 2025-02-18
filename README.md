# Server Preparation Playbook

## Description
This Ansible playbook automates the preparation of a server by applying multiple roles related to encryption, CPU configuration, and network setup. It ensures that necessary configurations are applied and provides a summary of executed roles, including success and failure tracking.

**Warning:** When encrypting a disk or partition, there is a risk of data loss. This script should only be executed on a fresh server with no important data.

## Features
- Encrypts a specified disk and partition.
- Configures CPU settings.
- Configures network settings.
- Handles role execution tracking.
- Provides a summary of executed roles.
- Supports conditional reboot if required.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Tested on Ubuntu-based systems.
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).
  - Required roles (`encrypt_disk`, `encrypt_partition`, `cpu_config`, `network_config`, `cpu_info`).

## Inventory Configuration
The `inventory/hosts.yml` file should define the target servers and required variables. Example:

```yaml
all:
  hosts:
    MyLinuxServer:
      ansible_host: *.*.*.* # server IP address
      ansible_user: ubuntu # user name on the server
      ansible_ssh_private_key_file: ~/.ssh/PemKey.pem # link to private key 

      encrypt_luks_device__disk_name: /dev/xvdb # disk name which has to be encrypted
      encrypt_luks_device__partition_name: encrypted_xvdb # partition name, which will be created on the disk
      encrypt_luks_device__mount_point: /newdisk # mount point of that partition
      encrypt_luks_device__luks_password: !vault | # the encryption key (it is better to use the Ansible Vault file and not store it in repositories)
        $ANSIBLE_VAULT;1.1;AES256
        66363133373437386239643830333135613364366438633662313638383939396636653734353364
        6330633634303738660a323636393139623263343738383630653562653032343838373030393834
        65656663316136386430376365656461383063366231383738626138336431666438

      encrypt_partition__partition_name: /dev/xvda14 # partition name which has to be encrypted
      encrypt_partition__luks_password: !vault | # the encryption key (it is better to use the Ansible Vault file and not store it in repositories)
        $ANSIBLE_VAULT;1.1;AES256
        66363133373437386239643830333135613364366438633662313638383939396636653734353364
        6330633634303738660a323636393139623263343738383630653562653032343838373030393834
        65656663316136386430376365656461383063366231383738626138336431666438

```

## Usage
Run the playbook using the following command:

```bash
ansible-playbook playbooks/prepare_server.yml --ask-vault-pass
```

## Playbook Structure
### Roles Executed:
- **encrypt_disk**: Encrypts a specified disk.
- **encrypt_partition**: Encrypts a specified partition.
- **cpu_config**: Configures CPU settings.
- **network_config**: Configures network settings.
- **cpu_info**: Gathers CPU information.

## Playbook Variables
- `global_reboot_required`: A boolean flag that indicates if a system reboot is needed based on executed roles.

### Execution Flow:
1. Runs each role sequentially.
2. If a global reboot is required, the server reboots before continuing.
   - The playbook uses the `global_reboot_required` variable to determine whether a reboot is needed. Roles that require a system reboot set this variable to `true`.

These lists help administrators quickly assess the playbook's execution outcome and understand if a reboot is necessary.

## Troubleshooting
- Ensure the target machine has proper SSH access and the correct private key is used.
- Verify that Ansible has `become` privileges to execute system-level commands.
- If encryption roles fail, confirm that the provided LUKS passwords are correct.
- If the server fails to reboot, check system logs for potential timeout or misconfiguration.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin