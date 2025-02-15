# Encrypt Disk Role

## Description
This Ansible role encrypts a specified disk using LUKS, creates a filesystem on it, and mounts it at a designated location. It ensures the encrypted disk is persistently available across reboots by updating `/etc/crypttab` and `/etc/fstab`.

## Features
- Verifies if the disk exists before encryption.
- Checks if the disk is already encrypted to prevent redundant operations.
- Creates a LUKS-encrypted partition using a predefined key file.
- Formats the encrypted partition with the `ext4` filesystem.
- Mounts the encrypted disk at a specified mount point.
- Ensures persistent mounting across reboots.
- Marks the system for reboot if required.
- Tracks execution success and failures for easier troubleshooting.

## **⚠️ Warning**
Encrypting a disk will erase all existing data on it. This script should only be executed on a new or empty disk that does not contain important data.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Tested on Debian-based systems (Ubuntu, Debian).
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).
  - `cryptsetup` package must be installed.

## Reboot Requirement
This role sets the playbook variable global_reboot_required to true, which triggers a server reboot after all roles are executed. If you use this role in a playbook that does not include a global reboot mechanism, ensure that the server is manually rebooted after execution.

## Role Variables
- `encrypt_disk__disk_name`: The disk device to be encrypted (e.g., `/dev/xvdb`).
- `encrypt_disk__partition_name`: The name of the LUKS container.
- `encrypt_disk__mount_point`: The directory where the encrypted disk will be mounted.
- `encrypt_disk__luks_password`: The LUKS passphrase for encryption (should be stored securely, e.g., in Ansible Vault).

## Usage
Include this role in your playbook as follows:

```yaml
- name: Encrypt and mount a disk
  hosts: all
  become: true
  roles:
    - role: encrypt_disk
```

## Execution Flow
### Disk Encryption Process:
1. Checks if the specified disk exists.
2. Verifies whether the disk is already encrypted.
3. Creates a LUKS key file to store the encryption password securely.
4. Encrypts the disk with LUKS.
5. Opens the encrypted LUKS container.
6. Creates an `ext4` filesystem on the encrypted disk.
7. Creates and mounts the disk at the specified location.
8. Updates `/etc/crypttab` and `/etc/fstab` for persistence.
9. Marks the system for reboot if required.

## Troubleshooting
- Ensure the target disk exists and is not already encrypted.
- Verify that Ansible has `become` privileges to execute `cryptsetup` commands.
- If mounting fails, confirm the LUKS container is properly opened using `cryptsetup status`.
- A reboot may be required for `/etc/crypttab` and `/etc/fstab` changes to take effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin