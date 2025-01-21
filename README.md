# Ansible Role HestiaCP

This Ansible role, in addition to installing HestiaCP, provides the following features:

1. **Service configuration selection**: Customize components such as Apache, PHP-FPM, MySQL, and install additional PHP versions (e.g., 7.4, 8.0, 8.1).
2. **SSL certificate generation for hostname**: Automatically generate a certificate via LetsEncrypt.
3. **Preconfigured MySQL setups**: Choose from eight predefined configurations for servers with varying memory sizes (configs by <a href="https://github.com/t0rik/mysql-configs-samples/tree/master">torik</a>).
4. **Enhanced Bind (Named) configuration**: Optimize DNS settings for improved performance and stability.
5. **Default nameservers setup**: Set default nameservers for all users.
6. **Backup count configuration**: Define limits for the number of backups created per user.
7. **Fixes for known HestiaCP issues**: Address problems in versions 1.8.12 and earlier.

## Table of Contents

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)

## Requirements

For this role, the recommended server specifications are:

- **CPU**: 2 cores
- **RAM**: 2 GB
- **Disk**: 20 GB of free space
- **OS**: Ubuntu 22.04.5

You can find detailed requirements on the official [HestiaCP website](https://www.hestiacp.com).

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

## Dependencies

The role has no external dependencies.

## Example Playbook

Using the role:

```yaml
- hosts: servers
  roles:
    - { role: nikchester.ansible-role.hestiacp }
```

## License

The Hestia Control Panel is licensed under GPL v3 and is based on the VestaCP project.

## Author Information

This Ansible role was created in 2021 by Jitendra Bodmann and improved by Nikita Chaturov in 2025. If you have questions or suggestions, feel free to reach out via the project's GitHub.
