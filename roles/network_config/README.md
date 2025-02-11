# Network Configuration Role

## Description
This Ansible role renames the active network interface and updates the Netplan configuration for Ubuntu-based systems. The role ensures that the new network interface name is applied correctly and marks the system for reboot if required.

## Features
- Detects the active network interface.
- Renames the network interface if necessary.
- Updates and applies Netplan configuration for Ubuntu 18.04 and later.
- Provides debug output of the new network interface settings.
- Tracks execution success and failures for troubleshooting.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Ubuntu 18.04+ (Tested on Ubuntu-based systems).
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).
  - Netplan must be available for configuration changes.

## Role Variables
- `new_name_net_interface`: The new name to assign to the network interface (default: `net0`).

## Usage
Include this role in your playbook as follows:

```yaml
- name: Configure network interface
  hosts: all
  become: true
  roles:
    - role: network_config
```

## Execution Flow
### Network Interface Renaming:
1. Detects the active network interface.
2. Compares the detected interface name with `new_name_net_interface`.
3. If the name is different, updates and applies Netplan configuration.
4. Displays information about the renamed network interface.
5. Marks the system for reboot if required.

## Role Execution Tracking
This role maintains two lists to track execution:
- `roles_all`: Contains all executed tasks.
- `roles_failed`: Lists tasks that encountered errors.
- If any step fails, the failure is recorded, allowing for easier troubleshooting.

## Troubleshooting
- Ensure the system is running Ubuntu 18.04 or later before executing this role.
- Verify that Ansible has `become` privileges to modify `/etc/netplan/`.
- If network settings do not apply, check the generated Netplan configuration at `/etc/netplan/50-cloud-init.yaml`.
- A reboot is required for the new network settings to fully take effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

