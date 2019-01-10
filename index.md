This  guide is a quick start for administrators and users. For administrators, it describes how to 
- install and configure Owncloud
- enable users to connect to Owncloud servers using server IP addresses and port 8080
- add user accounts

For users, it explains how to connect to Owncloud servers using desktop or mobile clients

## For Administrators
### System Requirements
You must have the following components installed prior to installing and configuring Owncloud servers.
#### Operating System
* Red Hat Enterprise Linux 6 and 7 (64-bit only)
#### Database
* MySQL or MariaDB 5.5+
* Oracle 11g
* PostgreSQL
* SQLite
#### Webserver
* Apache 2.4 with prefork [Multi-Processing Module (MPM)](https://doc.owncloud.org/server/10.0/admin_manual/installation/source_installation.html#apache-mpm-label) and mod_php

### Installing and Configuring Owncloud Servers
#### Required Extensions
###### Enable the RHEL Server 7 repository
subscription-manager repos --enable rhel-server-rhscl-7-eus-rpms

###### Install the required packages
`sudo yum install httpd mariadb-server php72 php72-php \
  php72-php-gd php72-php-mbstring php72-php-mysqlnd`
  
Optional Extensions¶
`sudo yum install -y epel-release http://rpms.remirepo.net/enterprise/remi-release-7.rpm yum-utils \
  && sudo yum-config-manager --enable remi-php72 \
  && sudo yum update -y \
  && sudo yum install -y php72-pecl-apcu \
    redis php72-php-pecl-redis php72-php-ldap \
    mariadb-server mariadb`

### Enabling Users to Connect to Owncloud Servers Using IP addresses and port 8080

### Adding user accounts
Follow these steps to add new users:
1. Navigate to the User management page of your ownCloud Web UI.
2. Enter the new user’s Login Name and their initial Password.
3. Optionally, assign Groups memberships.
4. Click **Create**.

 
## For Users
### Connecting to Owncloud servers using desktop clients
### Connecting to Owncloud servers using mobile clients

## Troubleshooting
If you are having problems, see [General Troubleshooting](https://doc.owncloud.org/server/10.0/admin_manual/issues/general_troubleshooting.html) and the [Owncloud Forums](https://central.owncloud.org/).
