# Ansible osTicket
CI

An Ansible Role to install osTicket on Ubuntu servers. You must have a Web Server, php and MySQL or MariaDB on your server (see [example](#Examples) below).

## Requirements

You can find the updated prerequisites in [osTicket's official documentation](https://docs.osticket.com/en/latest/Getting%20Started/Installation.html).
For version osTicket 1.17.4:

* HTTP server running Microsoft® IIS or Apache
* PHP version 8.0 - 8.1 (8.1 recommended)
* mysqli extension for PHP
* MySQL database version 5.5
* gd, gettext, imap, json, mbstring, and xml extensions for PHP
* APC module enabled and configured for PHP

You can use supertarto.apache, supertarto.mariadb and supertarto.php to install those beforehand with ansible

## Tested platforms

* Ubuntu 22.04
* Ubuntu 20.04
* Ubuntu 18.04

## Role variables


## Examples

## Installation
```
ansible-galaxy install lucas_stofaleti.osticket
```

## License
MIT / BSD