# Encrypt Partition Role

## Description
This Ansible role encrypts a specified partition using LUKS, ensuring secure data storage. The encrypted partition is configured for persistent availability across reboots by updating `/etc/crypttab`.

## Features
- Checks if the partition exists before encryption.
- Prevents re-encryption of an already encrypted partition.
- Creates a LUKS-encrypted container using a predefined key file.
- Updates `/etc/crypttab` to ensure persistence across reboots.
- Marks the system for reboot if required.
- Tracks execution success and failures for easier troubleshooting.

## **⚠️ Warning**
Encrypting a partition will erase all existing data on it. This script should only be executed on a partition that does not contain important data.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Tested on Debian-based systems (Ubuntu, Debian).
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).
  - `cryptsetup` package must be installed.

## Role Variables
- `encrypt_partition__partition_name`: The partition to be encrypted (e.g., `/dev/xvda14`).
- `encrypt_partition__luks_password`: The LUKS passphrase for encryption (should be stored securely, e.g., in Ansible Vault).

## Usage
Include this role in your playbook as follows:

```yaml
- name: Encrypt and configure a partition
  hosts: all
  become: true
  roles:
    - role: encrypt_partition
```

## Execution Flow
### Partition Encryption Process:
1. Checks if the specified partition exists.
2. Verifies whether the partition is already encrypted.
3. Creates a LUKS key file to securely store the encryption password.
4. Encrypts the partition with LUKS.
5. Opens the encrypted LUKS container.
6. Retrieves the UUID of the encrypted partition.
7. Updates `/etc/crypttab` to ensure persistence.
8. Marks the system for reboot if required.

## Role Execution Tracking
This role maintains two lists to track execution:
- `roles_all`: Contains all executed tasks.
- `roles_failed`: Lists tasks that encountered errors.
- If any step fails, the failure is recorded, allowing for easier troubleshooting.

## Troubleshooting
- Ensure the target partition exists and is not already encrypted.
- Verify that Ansible has `become` privileges to execute `cryptsetup` commands.
- If the partition is already encrypted, manually inspect its state using `lsblk -o NAME,FSTYPE`.
- A reboot may be required for `/etc/crypttab` changes to take effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

