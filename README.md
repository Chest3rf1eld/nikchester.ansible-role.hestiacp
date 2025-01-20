Ansible Role HestiaCP
=========

This ansible role in addition to installing HesiaCP allows you to:
1. Select the configuration of services for HestiaCP
2. Add the required php versions
3. create a hostname certificate
4. Install one of the eight ready-made configs for MySQL
5. Install the advanced configuration for Bind (Named)
5. Set which nameservers will be the default for each user
6. Set the number of backups for each user
7. Fixes some bugs of HestiaCP version 1.8.12 and below 
And all this in one go. 

Contents
-----------
- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)

Requirements
------------
No special requirements.

The requirements for the hosts are like those of hestiacp. You can find the requirements for the hosts on <a href="https://www.hestiacp.com">the project homepage</a>.

Role Variables
--------------
Configurations as in defaults/main.yml

    ```
    # server configs
    # the hostname must already have been setup with an 'A' record
    server_hostname: server.example.com
    
    # hestia admin configs
    # recommended to put this into vaul.yml of host_vars or group_vars
    # Initial details of hestiacp admin user
    hestia_admin_mail: admin@example.com
    hestia_admin_pw: P@ssword123
    
    # Install flags for hestiacp install script
    # values 'yes' or 'no'
    # “named” is a ‘bind’ service
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
    
    # Install additional php versions.
    # Checks if it is already is installed, so you can list all desired php versions.
    # Only gets installed when installer_flag is set to: --multiplephp no
    hestia_additional_php:
      - 7.4
      - 8.0
      - 8.1
    
    # Should hestia generate a letsencrypt cert for the hostname
    hestia_generate_ssl: true
    
    # Timeout parameters for async install script task
    hestia_timeout_install: 1000
    hestia_poll_interval_install: 15
    
    # Configs
    mysql_conf: templates/mysql-configs-samples/4GB-RAM-my.cnf
    bind_conf: templates/named.conf.options.j2
    
    # Admin package
    package: templates/default.pkg.j2
    package_backups: 1
    ns: ns1.example.com, ns2.example.com
    ```

Dependencies
------------

None.

Example Playbook
----------------

Nothing special when it comes to usage of this role. Make sure you have overwritten the variables of defaults/main.yml with your desired values.

    - hosts: servers
      roles:
         - { role: nikchester.ansible-role.hestiacp }

License
-------

The license of Hestia Control Panel applies. The Hestia Control Panel is licensed under GPL v3 license, and is based on the VestaCP project.

Author Information
------------------

This Ansible role was created in 2021 by <a href=“https://github.com/jeazyee”>Jitendra Bodmann</a>, owner of <a href=“https://www.wedima.de”>wedima</a> and improved by Nikita Chaturov.
