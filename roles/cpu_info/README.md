# CPU Info Role

## Description
This role gathers and displays CPU-related information from the target system. It checks for key CPU details and identifies whether Hyper-Threading (for Intel CPUs) or SMT (Simultaneous Multithreading for AMD CPUs) is enabled. Additionally, it provides clear diagnostics if the SMT status file is missing, with possible causes and recommendations.

## Features
- Collects CPU information using the `lscpu` command.
- Checks for the existence of the SMT status file: `/sys/devices/system/cpu/smt/active`.
- Displays whether Hyper-Threading (Intel) or SMT (AMD) is enabled or disabled.
- Provides detailed error messages if the SMT file is missing, with reasons and potential fixes.

## Requirements
- **Ansible Version**: 2.18.2 (development and testing conducted on this version)
- **Supported Platforms**:
  - Ubuntu 24.04.1 (tested and verified)

## Example Playbook

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

## Notes
- Ensure the target machine has `lscpu` installed.
- Run the role with `become: true` to access system-level files.
- Ensure the system has proper permissions to access `/sys/devices/system/cpu/smt/active`.
- Some older CPUs or custom Linux kernels might not support SMT/Hyper-Threading or lack the corresponding file.
