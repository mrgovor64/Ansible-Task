# CPU Configuration Role

## Description
This Ansible role configures CPU performance settings on a Linux system by enabling frequency scaling. These settings help achieve consistent CPU behavior by reducing power-saving features that may introduce latency.

## Features
- Ensures CPU frequency scaling is supported and sets the governor to `performance`.
- Configures `udev` rules to persist CPU governor settings.
- Marks the system for reboot if changes require a restart.
- Tracks execution success and failures for easier debugging.

## Requirements
- **Ansible Version**: Ensure that Ansible is installed on the control node.
- **Supported Platforms**: Tested on Debian-based systems (Ubuntu, Debian).
- **Dependencies**:
  - SSH access to the target server.
  - The target server must allow Ansible to run with elevated privileges (`become: true`).

## Reboot Requirement
This role sets the playbook variable global_reboot_required to true, which triggers a server reboot after all roles are executed. If you use this role in a playbook that does not include a global reboot mechanism, ensure that the server is manually rebooted after execution.

## Usage
Include this role in your playbook as follows:

```yaml
- name: Configure CPU settings
  hosts: all
  become: true
  roles:
    - role: cpu_frequency_scaling
```

## Execution Flow
### Frequency Scaling:
1. Checks if CPU supports frequency scaling.
2. Sets the CPU governor to `performance` using `udev` rules.
3. Reloads `udev` rules to apply changes.
4. Marks the system for reboot if needed.

## Troubleshooting
- Ensure the system supports CPU frequency scaling before running this role.
- Verify that Ansible has `become` privileges to modify `/etc/default/grub` and `udev` rules.
- A reboot is required for changes to take full effect.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

