# Ansible Role HestiaCP

This Ansible role, in addition to installing HestiaCP, provides the following features:

1. **Service Configuration Selection**: Customize components such as Apache, PHP-FPM, MySQL, and install additional PHP versions (e.g., 7.4, 8.0, 8.1).
2. **SSL Certificate Generation for Hostname**: Automatically generate a certificate via LetsEncrypt.
3. **Preconfigured MySQL Setups**: Choose from eight predefined configurations for servers with varying memory sizes (configs by [torik](https://github.com/t0rik/mysql-configs-samples/tree/master)).
4. **Enhanced Bind (Named) Configuration**: Optimize DNS settings for improved performance and stability.
5. **Default Nameservers Setup**: Set default nameservers for all users.
6. **Backup Count Configuration**: Define limits for the number of backups created per user.
7. **Fixes for Known HestiaCP Issues**: Address problems in versions 1.8.12 and earlier.

---

## Table of Contents

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Variable Validation](#variable-validation)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)

---

## Requirements

For this role, the recommended server specifications are:

- **CPU**: 2 cores
- **RAM**: 2 GB
- **Disk**: 20 GB of free space
- **OS**: Ubuntu 22.04.5

You can find detailed requirements on the official [HestiaCP website](https://www.hestiacp.com).

---

## Role Variables

Key settings are defined in the `defaults/main.yml` file:

```yaml
# Server configurations
# The hostname must already have an 'A' record configured
server_hostname: server.example.com

# Hestia admin configurations
# It is recommended to store these details in a secure vault.yml
hestia_admin_mail: "Your_Email"
# Specify a unique and secure password. Including example passwords in documentation is not recommended.
hestia_admin_pw: "Your_Secure_Password"

# Installation flags for HestiaCP
hestia_install_flags:
  apache: 'yes'
  phpfpm: 'yes'
  multiphp: 'no'
  vsftpd: 'yes'
  proftpd: 'no'
  named: 'yes'
  mysql: 'yes'
  postgresql: 'no'
  exim: 'no'
  dovecot: 'no'
  sieve: 'no'
  clamav: 'no'
  spamassassin: 'no'
  iptables: 'yes'
  fail2ban: 'yes'
  quota: 'yes'
  api: 'yes'
  port: '8083'
  lang: 'en'

# Additional PHP versions
hestia_additional_php:
  - 7.4
  - 8.0
  - 8.1

# LetsEncrypt certificate generation
hestia_generate_ssl: true

# Timeout settings for installation
hestia_timeout_install: 1000
hestia_poll_interval_install: 15

# Configurations
mysql_conf: templates/mysql-configs-samples/2GB-RAM-my.cnf # Available configurations for: 1, 2, 4, 8, 16, 32, 64, 128 GB
bind_conf: templates/named.conf.options.j2

# Admin package settings
package: templates/default.pkg.j2
package_backups: 1
ns: ns1.example.com, ns2.example.com
```

---

## Variable Validation

Before executing the role, a built-in mechanism ensures that all required variables are properly defined and meet the expected criteria. This includes:

- Validation of `server_hostname` for non-empty and valid format.
- Checking that `hestia_admin_mail` contains a valid email address.
- Ensuring `hestia_admin_pw` is secure and meets minimum complexity requirements.
- Verifying that `hestia_install_flags` only contain valid options (`yes` or `no`).
- Confirming the presence of configurations like `mysql_conf` and `bind_conf`.

If any variable fails validation, the role stops execution and provides an informative error message.

---

## Dependencies

The role has no external dependencies.

---

## Example Playbook

Using the role:

```yaml
- hosts: servers
  roles:
    - { role: nikchester.ansible-role.hestiacp }
```

---

## License

The Hestia Control Panel is licensed under GPL v3 and is based on the VestaCP project.

---

## Author Information

This Ansible role was created in 2021 by Jitendra Bodmann and improved by Nikita Chaturov in 2025. If you have questions or suggestions, feel free to reach out via the project's GitHub.

