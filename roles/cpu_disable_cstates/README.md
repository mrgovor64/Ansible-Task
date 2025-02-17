# CPU Configuration Role

## Description
This Ansible role configures CPU performance settings on a Linux system by disabling CPU C-states to optimize performance. These settings help achieve consistent CPU behavior by reducing power-saving features that may introduce latency.

## Features
- Disables CPU C-states to prevent unwanted power-saving modes.
- Updates the GRUB configuration for permanent C-state settings.
- Marks the system for reboot if changes require a restart.
- Tracks execution success and failures for easier debugging.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Tested on Debian-based systems (Ubuntu, Debian).
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).
  - GRUB must be installed on the system to disable C-states.

## Reboot Requirement
This role sets the playbook variable global_reboot_required to true, which triggers a server reboot after all roles are executed. If you use this role in a playbook that does not include a global reboot mechanism, ensure that the server is manually rebooted after execution.

## Usage
Include this role in your playbook as follows:

```yaml
- name: Configure CPU settings
  hosts: all
  become: true
  roles:
    - role: cpu_disable_cstates
```

## Execution Flow

### Disable C-states:
1. Checks if C-states are already disabled.
2. Updates the GRUB configuration to disable C-states.
3. Runs `update-grub` (for Debian-based systems).
4. Marks the system for reboot if needed.

## Troubleshooting
- Verify that Ansible has `become` privileges to modify `/etc/default/grub`.
- If GRUB updates fail, check if the system is using another bootloader.
- A reboot is required for changes to take full effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

