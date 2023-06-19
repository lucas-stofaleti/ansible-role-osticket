# Ansible osTicket
[![CI](https://github.com/lucas-stofaleti/ansible-role-osticket/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/lucas-stofaleti/ansible-role-osticket/actions/workflows/ci.yml)

An Ansible Role to install osTicket on Ubuntu servers. You must have Apache, PHP and MySQL on your server (see [example](#examples) below).

## Requirements

You can find the updated prerequisites in [osTicket's official documentation](https://docs.osticket.com/en/latest/Getting%20Started/Installation.html).
For version osTicket 1.17.4:

* HTTP server running Microsoft® IIS or Apache
* PHP version 8.0 - 8.1 (8.1 recommended)
* mysqli extension for PHP
* MySQL database version 5.5
* gd, gettext, imap, json, mbstring, and xml extensions for PHP
* APC module enabled and configured for PHP

You can use geerlingguy.apache, geerlingguy.mysql and geerlingguy.php to install those beforehand with ansible.

## Tested platforms

* Ubuntu 22.04
* Ubuntu 20.04
* Ubuntu 18.04

## Role variables

The osTicket version and download url.
```yml
osticket_version: "v1.17.4"
osticket_download_url: "https://github.com/osTicket/osTicket/releases/download/{{ osticket_version }}/osTicket-{{ osticket_version }}.zip"
```

osTicket installation path, config sample and config file. If you are migrating to a new server, you can replace the osticket_config_sample variable with your already configured file.
```yml
osticket_install_path: "/var/www/osticket"
osticket_config_sample: "{{ osticket_install_path }}/upload/include/ost-sampleconfig.php"
osticket_config_file: "{{ osticket_install_path }}/upload/include/ost-config.php"
```

User and group that own the files.
```yml
osticket_web_owner: "www-data"
osticket_web_group: "www-data"
```

Specify the list of plugins you want to install.
```yml
osticket_plugin_url: "https://s3.amazonaws.com/downloads.osticket.com/plugin"
osticket_plugin_folder: "{{ osticket_install_path }}/upload/include/plugins"
osticket_plugins: []
```

Set to true if you want to update your osTicket. If set to false and any version of osTicket is installed, no version will be installed.
```yml
osticket_update: false
```

## Examples
```yml
---
- hosts: all
  become: yes
  vars:
    osticket_version: "v1.18"
    osticket_plugins:
      - auth-oauth2
      - auth-2fa
    mysql_root_password: super-secure-password
    mysql_databases:
      - name: osticket
    mysql_users:
      - name: osticket
        host: "%"
        password: super-secure-password
        priv: "osticket.*:ALL"
    apache_remove_default_vhost: true
    apache_vhosts:
      - servername: "osticket.test"
        documentroot: "/var/www/osticket/upload"
        serveralias: "www.osticket.test"
        extra_parameters: |
          ErrorLog ${APACHE_LOG_DIR}/osticket-error.log
          CustomLog ${APACHE_LOG_DIR}/osticket-access.log combined
    php_packages:
      - php8.1
      - libapache2-mod-php8.1
      - php8.1-mysql
      - php8.1-cgi
      - php8.1-fpm
      - php8.1-cli
      - php8.1-curl
      - php8.1-gd
      - php8.1-imap
      - php8.1-mbstring
      - php8.1-intl
      - php8.1-apcu
      - php8.1-common
      - php8.1-bcmath
      - php8.1-xml

  pre_tasks:
    - name: Add PHP source
      ansible.builtin.apt_repository:
        repo: ppa:ondrej/php

  tasks:
    - name: Include Apache role
      include_role:
        name: geerlingguy.mysql

    - name: Include Apache role
      include_role:
        name: geerlingguy.apache

    - name: Include PHP role
      include_role:
        name: geerlingguy.php

    - name: Include osTicket role
      include_role:
        name: lucas_stofaleti.osticket
```       

## Installation
```
ansible-galaxy install lucas_stofaleti.osticket
```

## License
MIT / BSD