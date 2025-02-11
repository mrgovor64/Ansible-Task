# CPU Configuration Role

## Description
This Ansible role configures CPU performance settings on a Linux system by enabling frequency scaling and disabling CPU C-states to optimize performance. These settings help achieve consistent CPU behavior by reducing power-saving features that may introduce latency.

## Features
- Ensures CPU frequency scaling is supported and sets the governor to `performance`.
- Configures `udev` rules to persist CPU governor settings.
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

## Usage
Include this role in your playbook as follows:

```yaml
- name: Configure CPU settings
  hosts: all
  become: true
  roles:
    - role: cpu_config
```

## Execution Flow
### Frequency Scaling:
1. Checks if CPU supports frequency scaling.
2. Sets the CPU governor to `performance` using `udev` rules.
3. Reloads `udev` rules to apply changes.
4. Marks the system for reboot if needed.

### Disable C-states:
1. Checks if C-states are already disabled.
2. Updates the GRUB configuration to disable C-states.
3. Runs `update-grub` (for Debian-based systems).
4. Marks the system for reboot if needed.

## Role Execution Tracking
This role maintains two lists to track execution:
- `roles_all`: Contains all executed tasks.
- `roles_failed`: Lists tasks that encountered errors.
- If any step fails, the failure is recorded, allowing for easier troubleshooting.

## Troubleshooting
- Ensure the system supports CPU frequency scaling before running this role.
- Verify that Ansible has `become` privileges to modify `/etc/default/grub` and `udev` rules.
- If GRUB updates fail, check if the system is using another bootloader.
- A reboot is required for changes to take full effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

