# Baseline Linux Setup Role

This Ansible role provides comprehensive baseline configuration for Linux systems, supporting both Debian and RedHat distributions.

## Features

- **System Updates**: Ensures system packages are up to date
- **Security Hardening**: Basic security configurations
- **User Management**: Creates baseline users and groups
- **SSH Configuration**: Secure SSH server settings
- **Firewall Setup**: Basic firewall configuration
- **System Monitoring**: Basic monitoring tools installation
- **Logging Configuration**: Enhanced logging setup
- **Network Configuration**: Basic network optimizations
- **Package Management**: Essential packages installation

## Supported Distributions

- **Debian/Ubuntu**: Uses apt package manager
- **RedHat/CentOS/Rocky Linux**: Uses dnf/yum package manager

## Variables

### Required Variables

- `baseline_admin_user`: Username for the admin user (default: admin)
- `baseline_admin_password`: Password for the admin user (encrypted)
- `baseline_ssh_key`: SSH public key for admin user

### Optional Variables

- `baseline_timezone`: System timezone (default: UTC)
- `baseline_hostname`: System hostname
- `baseline_install_monitoring`: Install monitoring tools (default: true)
- `baseline_configure_firewall`: Configure firewall (default: true)
- `baseline_configure_ssh`: Configure SSH server (default: true)

## Usage

```yaml
- hosts: servers
  roles:
    - baseline
  vars:
    baseline_admin_user: admin
    baseline_admin_password: "{{ vault_admin_password }}"
    baseline_ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
    baseline_timezone: America/New_York
    baseline_hostname: "{{ inventory_hostname }}"
```

## Security Notes

- Always use encrypted passwords with Ansible Vault
- Review and customize security settings for your environment
- SSH keys should be properly secured
- Firewall rules should be tailored to your network requirements 