# CPU Info Role

## Description
This Ansible role gathers and displays CPU-related information from the target system. It checks for key CPU details and determines whether Hyper-Threading (for Intel CPUs) or Simultaneous Multithreading (SMT for AMD CPUs) is enabled. Additionally, it provides diagnostics if the SMT status file is missing, offering possible causes and recommendations.

## Features
- Collects CPU information using the `lscpu` command.
- Checks for the existence of the SMT status file: `/sys/devices/system/cpu/smt/active`.
- Displays whether Hyper-Threading (Intel) or SMT (AMD) is enabled or disabled.
- Provides detailed error messages if the SMT file is missing, with reasons and potential solutions.

## Requirements
- **Ansible Version**: 2.18.2 (Development and testing conducted on this version)
- **Supported Platforms**:
  - Ubuntu 24.04.1 (Tested and verified)
- **Dependencies**:
  - The `lscpu` command must be available on the target system.
  - The role must be executed with `become: true` to access system-level files.

## Role Variables
This role uses the following variables to track execution status:
- `roles_all`: A list containing all roles that were executed.
- `roles_failed`: A list containing roles that encountered an error during execution.

These variables are useful for tracking execution results at the end of a playbook, allowing users to see which roles ran successfully and which ones failed.

## Usage
Include this role in your playbook as follows:

```yaml
- name: Run CPU Info Role
  hosts: all
  become: true
  roles:
    - role: cpu_info
```

## Sample Output
### When SMT File Exists:
```
TASK [Gather CPU information] *****************************
ok: [server1]

TASK [Display CPU information] ***************************
ok: [server1] => {
    "msg": [
        "Architecture: x86_64",
        "CPU(s): 4",
        "Thread(s) per core: 2"
    ]
}

TASK [Display Hyper-Threading / SMT status] ***************
ok: [server1] => {
    "msg": "Hyper-Threading / SMT is enabled"
}
```

### When SMT File Is Missing:
```
TASK [SMT status file is missing] *************************
fatal: [server1]: FAILED! => {
    "msg": [
        "Error: The file /sys/devices/system/cpu/smt/active is missing!",
        "Possible reasons:",
        "- Your CPU does not support SMT / Hyper-Threading.",
        "- SMT is disabled in the BIOS/UEFI settings.",
        "- The Linux kernel does not support SMT control.",
        "- The system was booted with the `nosmt` kernel parameter.",
        "- Insufficient permissions."
    ]
}
```

## Troubleshooting
- Ensure that the `lscpu` command is installed on the target system.
- Verify that the role is executed with `become: true` to avoid permission issues.
- If the SMT file is missing, check BIOS settings and kernel parameters.
- Some older CPUs or custom Linux kernels may not support SMT/Hyper-Threading.

## Extending the Role
If you need to extend this role, consider adding tasks for:
- Checking additional CPU features (e.g., virtualization support).
- Logging results to an external system.
- Customizable thresholds and alerts.

## License
MIT License

## Author
Maintained by Vladimir Govorukhin

