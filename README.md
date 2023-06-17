# Ansible osTicket
CI

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

## Examples

## Installation
```
ansible-galaxy install lucas_stofaleti.osticket
```

## License
MIT / BSD