# Network Configuration Role

## Overview
This Ansible role detects the active network interface, renames it, updates the network configuration (currently for Ubuntu using Netplan), and marks the system for reboot if necessary. If the role cannot apply any configuration (due to an unsupported OS or version), it will fail gracefully without stopping the entire playbook.

## Features
- **Detects the current network interface** used for internet access.
- **Renames the interface** to a specified name (default: `net0`).
- **Updates Netplan configuration** for Ubuntu-based systems (18.04+).
- **Marks the system for reboot** if necessary.
- **Fails gracefully** if no matching configuration is found.

## Requirements
- Ansible 2.18.2+
- Ubuntu 18.04+ (Netplan-based networking)
- System must support `ip route` and `netplan apply` commands

## Role Variables
| Variable | Default Value | Description |
|----------|---------------|-------------|
| `new_name_net_interface` | `net0` | The new name for the network interface. |
| `global_reboot_required` | `true` | Marks system for reboot after renaming the interface. |
| `network_config_applied` | `false` | Internal flag to track if configuration was applied. |


## File Structure
```
roles/
  network_config/
    tasks/
      main.yml
      update_and_apply_config.yml
    templates/
      netplan.yaml.j2
    defaults/
      main.yml
```

## Usage
### Example Playbook
```yaml
- name: Configure network
  hosts: all
  become: true
  vars:
    global_reboot_required: false
  roles:
    - network_config

  post_tasks:
    - name: Server reboot
      reboot:
        reboot_timeout: 300
      when: global_reboot_required | bool
```

## How It Works
1. **Detects the active network interface** using `ip route`.
2. **Renames the interface** to `net0` (or custom name from `new_name_net_interface`).
3. **Updates and applies Netplan configuration** for Ubuntu systems.
4. **Displays network information** after changes.
5. **Marks the system for reboot** to ensure persistent changes.

## Applying Changes
After running this role, if the network interface was renamed, a reboot will be required. You can apply the changes by running:

## Future Improvements
- Support for multiple Linux distributions (e.g., Debian, RHEL, CentOS)
- Extend configuration handling for `systemd-networkd` and `ifupdown`

## Author
Maintained by Vladimir Govorukhin

## License
MIT License
