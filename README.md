hosts.yml example:
```
all:
  hosts:
    LinuxServer1:
      ansible_host: 192.168.0.10
      ansible_user: admin
      ansible_ssh_private_key_file: ~/pemkey
```