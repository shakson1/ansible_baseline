# Ansible Baseline Linux Setup

A comprehensive, production-ready Ansible role for baseline configuration and hardening of Linux servers. Supports both **Debian/Ubuntu** and **RedHat/CentOS/Rocky** distributions.

---

## 🚀 Features

- **System Updates**: Keeps your system packages up to date.
- **Security Hardening**: SSH hardening, firewall, fail2ban, SELinux (RedHat).
- **User Management**: Creates a secure admin user with SSH key and sudo access.
- **Monitoring**: Installs and configures system, disk, and network monitoring scripts.
- **Logging**: Enhanced rsyslog and logrotate configuration.
- **Network Optimization**: Kernel and sysctl tuning, NTP/Chrony setup.
- **Idempotent**: Safe to run multiple times.

---

## 📦 Supported Platforms

- Debian, Ubuntu (apt)
- RedHat, CentOS, Rocky Linux (dnf/yum)

---

## 🗂️ Project Structure

```
ansible_baseline/
├── ansible.cfg
├── playbook.yml
├── test.yml
├── requirements.yml
├── inventory/
│   └── hosts.yml
├── group_vars/
│   └── all/
│       └── vault.yml
├── roles/
│   └── baseline/
│       ├── defaults/
│       ├── handlers/
│       ├── meta/
│       ├── tasks/
│       ├── templates/
│       ├── vars/
│       └── README.md
└── README.md
```

---

## ⚡ Quick Start

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ansible_baseline
```

### 2. Configure Inventory

Edit `inventory/hosts.yml` with your server details.

### 3. Set Vault Password

Encrypt sensitive variables:

```bash
ansible-vault create group_vars/all/vault.yml
```

Add:

```yaml
vault_admin_password: "your_secure_password"
```

### 4. Run the Playbook

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

---

## 🔧 Configuration

### Required Variables

| Variable                  | Description                        | Example                        |
|---------------------------|------------------------------------|--------------------------------|
| baseline_admin_user       | Admin username                     | admin                          |
| baseline_admin_password   | Admin password (vault)             | vault_admin_password           |
| baseline_ssh_key          | SSH public key for admin           | lookup('file', '~/.ssh/id_rsa.pub') |

### Optional Variables

| Variable                        | Description                        | Default/Example                |
|----------------------------------|------------------------------------|-------------------------------|
| baseline_timezone                | System timezone                    | UTC, America/New_York         |
| baseline_hostname                | System hostname                    | inventory_hostname            |
| baseline_ssh_port                | SSH port                           | 22                            |
| baseline_firewall_allowed_ports  | List of allowed ports              | [22, 80, 443]                 |
| baseline_install_monitoring      | Install monitoring tools           | true                          |
| baseline_configure_firewall      | Configure firewall                 | true                          |
| baseline_configure_ssh           | Configure SSH server               | true                          |
| baseline_configure_logging       | Configure logging                  | true                          |
| baseline_configure_network       | Configure network                  | true                          |
| baseline_update_system           | Update system packages             | true                          |

See `roles/baseline/defaults/main.yml` for all options.

---

## 🛡️ Security Features

- **SSH**: Disables root login, password auth, X11/agent forwarding, enforces key-based auth.
- **Firewall**: UFW (Debian) or firewalld (RedHat) with configurable ports.
- **Fail2ban**: Protects against brute-force attacks.
- **SELinux**: Enforced on RedHat systems.
- **Audit Logging**: Comprehensive rules for RedHat.

---

## 📊 Monitoring & Logging

- **System, Disk, Network Monitoring**: Custom scripts in `/usr/local/bin/monitoring/` with logs in `/var/log/monitoring/`.
- **Logrotate**: Rotates system and custom logs.
- **Rsyslog**: Enhanced configuration for security and compliance.

---

## 🧪 Testing

A test playbook is provided:

```bash
ansible-playbook test.yml --check
```

---

## 📝 Example Playbook

```yaml
- hosts: all
  become: yes
  roles:
    - baseline
  vars:
    baseline_admin_user: admin
    baseline_admin_password: "{{ vault_admin_password }}"
    baseline_ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
    baseline_timezone: America/New_York
    baseline_ssh_port: 2222
    baseline_firewall_allowed_ports:
      - 22
      - 80
      - 443
      - 8080
```

---

## 🛠️ Troubleshooting

- **SSH Issues**: Check firewall, SSH key, and port.
- **Package Failures**: Check network, disk space, and repositories.
- **Service Failures**: Use `systemctl status <service>` and check logs.

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch
3. Make changes and add tests
4. Submit a pull request

---

## 📄 License

MIT License

---

## 🙋 Support

- Review logs and troubleshooting section
- Open an issue on GitHub

---

This role provides a secure, robust, and extensible baseline for any Linux server deployment.  
**Happy automating!** 