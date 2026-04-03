# jkirk.proxmox_cloud_init

A simple role to set up cloud-init on Proxmox PVE hosts.

## Requirements

N/A

## Role Variables

N/A

## Dependencies

Create the following file in your repository: `files/cloud-init.yml`:

```
#cloud-config

# defaults
timezone: "Europe/Vienna"

# packages:
package_upgrade: true
packages:
  - qemu-guest-agent
  - zsh

apt:
  disable_suites: [backports, $RELEASE-backports]

runcmd:
  - systemctl start qemu-guest-agent

# users:
chpasswd:
  expire: False

hostname: cloud-init-generic

users:
  - name: johndoe
    gecos: John Doe
    groups: [adm, sudo]
    sudo: ["ALL=(ALL) NOPASSWD:ALL"]
    shell: /usr/bin/zsh
    ssh_authorized_keys:
      - ssh-rsa [PUBLICKEY]
  - name: janedoe
    gecos: Jane Doe
    groups: [adm, sudo]
    sudo: ["ALL=(ALL) NOPASSWD:ALL"]
    shell: /usr/bin/zsh
    ssh_authorized_keys:
      - ssh-rsa [PUBLICKEY]

## Example Playbook

`site-proxmox.yml`:
```
---
- name: Set up cloud-init
  hosts: proxmox
  become: yes
  roles:
    - proxmox_cloud_init
```

## License

MIT

## Author Information

Darshaka Pathirana - <https://synpro.solutions>
