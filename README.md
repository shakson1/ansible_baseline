# Ansible Baseline Linux Setup

A comprehensive, production-ready Ansible role for baseline configuration and hardening of Linux servers. Supports both **Debian/Ubuntu** and **RedHat/CentOS/Rocky** distributions.

---

## 🚀 Features

- **System Updates**: Keeps your system packages up to date.
- **Security Hardening**: SSH hardening, firewall, fail2ban, SELinux (RedHat), AIDE, password policies.
- **User Management**: Creates a secure admin user with SSH key and sudo access.
- **Monitoring**: Installs and configures system, disk, and network monitoring scripts.
- **Advanced Monitoring**: Prometheus, Grafana Agent, Telegraf, Collectd, Netdata support.
- **Logging**: Enhanced rsyslog and logrotate configuration with log aggregation.
- **Network Optimization**: Kernel and sysctl tuning, NTP/Chrony setup.
- **Backup & Recovery**: Automated backup scripts with retention policies.
- **Validation**: Comprehensive validation and error handling.
- **Testing**: Enhanced testing capabilities with detailed reports.
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
| baseline_security_hardening      | Apply additional security hardening | true                          |
| baseline_advanced_monitoring     | Install advanced monitoring tools  | false                         |
| baseline_backup_enabled          | Enable backup functionality        | true                          |
| baseline_log_aggregation         | Enable log aggregation            | false                         |
| baseline_alert_email             | Email for alerts                  | admin@localhost               |
| baseline_alert_webhook           | Webhook URL for alerts            | ""                            |

See `roles/baseline/defaults/main.yml` for all options.

---

## 🛡️ Security Features

- **SSH**: Disables root login, password auth, X11/agent forwarding, enforces key-based auth.
- **Firewall**: UFW (Debian) or firewalld (RedHat) with configurable ports.
- **Fail2ban**: Protects against brute-force attacks.
- **SELinux**: Enforced on RedHat systems.
- **Audit Logging**: Comprehensive rules for RedHat.
- **AIDE**: File integrity monitoring.
- **Password Policies**: Enforced password complexity and aging.
- **Kernel Hardening**: Security-focused kernel parameters.
- **AppArmor**: Mandatory access control (Debian).
- **Security Monitoring**: Automated security event detection.

---

## 📊 Monitoring & Logging

- **System, Disk, Network Monitoring**: Custom scripts in `/usr/local/bin/monitoring/` with logs in `/var/log/monitoring/`.
- **Advanced Monitoring**: Prometheus Node Exporter, Grafana Agent, Telegraf, Collectd, Netdata.
- **Log Aggregation**: Centralized logging with rsyslog.
- **Alerting**: Email and webhook notifications for critical events.
- **Dashboard**: Web-based monitoring dashboard.
- **Logrotate**: Rotates system and custom logs.
- **Rsyslog**: Enhanced configuration for security and compliance.

---

## 🧪 Testing

A comprehensive test playbook is provided:

```bash
# Run full test suite
ansible-playbook test.yml --check

# Run with specific features
ansible-playbook test.yml --check -e "baseline_security_hardening=true"

# Run with advanced monitoring
ansible-playbook test.yml --check -e "baseline_advanced_monitoring=true"
```

### Test Features

- **Configuration Validation**: Validates all configuration parameters
- **File Permissions**: Checks critical file permissions
- **Network Connectivity**: Tests network connectivity
- **System Resources**: Monitors memory, disk, and CPU
- **Configuration Syntax**: Validates SSH and rsyslog configurations
- **Backup Scripts**: Tests backup and recovery functionality
- **Monitoring Scripts**: Validates monitoring tools
- **Test Reports**: Generates detailed test reports

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

## 💾 Backup & Recovery

### Automated Backups

The role includes comprehensive backup functionality:

```bash
# Manual system backup
/usr/local/bin/system_backup.sh

# Manual configuration backup
/usr/local/bin/config_backup.sh

# System recovery
/usr/local/bin/system_recovery.sh /var/backups/system/system_backup_20231201_020000.tar.gz

# Configuration recovery
/usr/local/bin/system_recovery.sh /var/backups/config/config_backup_20231201_030000.tar.gz config
```

### Backup Features

- **System Backups**: Complete system configuration and data
- **Configuration Backups**: SSH, firewall, logging, and service configurations
- **Automated Scheduling**: Daily backups with cron jobs
- **Retention Policies**: Configurable retention periods
- **Recovery Scripts**: Easy restoration of backups
- **Manifest Files**: Detailed backup information

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