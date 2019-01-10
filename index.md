This  guide is a quick start for administrators and users. For administrators, it describes how to 
- install and configure ownCloud
- enable users to connect to ownCloud servers using server IP addresses and port 8080
- add user accounts

For users, it explains how to connect to ownCloud servers using desktop or mobile clients

## For Administrators
### System Requirements
You must have the following components installed prior to installing and configuring ownCloud servers.
#### Operating System
* Red Hat Enterprise Linux 6 and 7 (64-bit only)
#### Database
* MySQL or MariaDB 5.5+
* Oracle 11g
* PostgreSQL
* SQLite
#### Webserver
* Apache 2.4 with prefork [Multi-Processing Module (MPM)](https://doc.owncloud.org/server/10.0/admin_manual/installation/source_installation.html#apache-mpm-label) and mod_php

### Installing and Configuring ownCloud Servers
#### Required Extensions
###### Enable the RHEL Server 7 repository
`subscription-manager repos --enable rhel-server-rhscl-7-eus-rpms`

###### Install the required packages
`sudo yum install httpd mariadb-server php72 php72-php \
  php72-php-gd php72-php-mbstring php72-php-mysqlnd`
  
##### Optional Extensions¶
`sudo yum install -y epel-release http://rpms.remirepo.net/enterprise/remi-release-7.rpm yum-utils \
  && sudo yum-config-manager --enable remi-php72 \
  && sudo yum update -y \
  && sudo yum install -y php72-pecl-apcu \
    redis php72-php-pecl-redis php72-php-ldap \
    mariadb-server mariadb`
    
### Install ownCloud
Follow these steps:
1. Download the archive of the latest ownCloud version:
2. Go to the ownCloud Download Page.
3. Go to **Download ownCloud Server > Download > Archive file for server owners** and download either the `tar.bz2` or `.zip` archive.
This downloads a file named `owncloud-x.y.z.tar.bz2` or `owncloud-x.y.z.zip` (where `x.y.z` is the version number).
4. Download its corresponding checksum file, e.g. owncloud-x.y.z.tar.bz2.md5, or owncloud-x.y.z.tar.bz2.sha256.
5. Verify the MD5 or SHA256 sum:

`md5sum -c owncloud-x.y.z.tar.bz2.md5 < owncloud-x.y.z.tar.bz2
sha256sum -c owncloud-x.y.z.tar.bz2.sha256 < owncloud-x.y.z.tar.bz2
md5sum  -c owncloud-x.y.z.zip.md5 < owncloud-x.y.z.zip
sha256sum  -c owncloud-x.y.z.zip.sha256 < owncloud-x.y.z.zip`
6. (Optional) You may also verify the PGP signature:

`wget https://download.owncloud.org/community/owncloud-x.y.z.tar.bz2.asc
wget https://owncloud.org/owncloud.asc
gpg --import owncloud.asc
gpg --verify owncloud-x.y.z.tar.bz2.asc owncloud-x.y.z.tar.bz2`
7. Extract the archive contents. 
8. Run the appropriate unpacking command for your archive type:
`tar -xjf owncloud-x.y.z.tar.bz2
unzip owncloud-x.y.z.zip`
This unpacks to a single owncloud directory. 
9. Copy the ownCloud directory to its final destination. When you are running the Apache HTTP server, you may safely install ownCloud in your Apache document root:
`
`cp -r owncloud /path/to/webserver/document-root
where `/path/to/webserver/document-root` is replaced by the document root of your Web server:

`cp -r owncloud /var/www`
On other HTTP servers, we recommend installing ownCloud outside of the document root.

### Configure Apache Web Server
On Debian, Ubuntu, and their derivatives, Apache installs with a useful configuration, so all you have to do is create a /etc/apache2/sites-available/owncloud.conf file with these lines in it, replacing the Directory and other file paths with your own file paths:

Alias /owncloud "/var/www/owncloud/"

<Directory /var/www/owncloud/>
  Options +FollowSymlinks
  AllowOverride All

 <IfModule mod_dav.c>
  Dav off
 </IfModule>

 SetEnv HOME /var/www/owncloud
 SetEnv HTTP_HOME /var/www/owncloud

</Directory>
Then create a symlink to /etc/apache2/sites-enabled:

ln -s /etc/apache2/sites-available/owncloud.conf /etc/apache2/sites-enabled/owncloud.conf
Additional Apache Configurations
For ownCloud to work correctly, we need the module mod_rewrite. Enable it by running:

a2enmod rewrite
Additional recommended modules are mod_headers, mod_env, mod_dir and mod_mime

a2enmod headers
a2enmod env
a2enmod dir
a2enmod mime
If you want to use the OAuth2 app, then mod_headers must be installed and enabled.

You must disable any server-configured authentication for ownCloud, as it uses Basic authentication internally for DAV services. If you have turned on authentication on a parent folder (via, e.g., an AuthType Basic directive), you can disable the authentication specifically for the ownCloud entry. Following the above example configuration file, add the following line in the <Directory section

Satisfy Any
When using SSL, take special note of the ServerName. You should specify one in the server configuration, as well as in the CommonName field of the certificate. If you want your ownCloud to be reachable via the internet, then set both of these to the domain you want to reach your ownCloud server.

Now restart Apache

service apache2 restart
If you’re running ownCloud in a sub-directory and want to use CalDAV or CardDAV clients make sure you have configured the correct Service discovery URLs.

Multi-Processing Module (MPM)
Apache prefork has to be used. Don’t use a threaded MPM like event or worker with mod_php, because PHP is currently not thread safe.

Enable SSL
You can use ownCloud over plain HTTP, but we strongly encourage you to use SSL/TLS to encrypt all of your server traffic, and to protect user’s logins and data in transit.

Apache installed under Ubuntu comes already set-up with a simple self-signed certificate. All you have to do is to enable the ssl module and the default site. Open a terminal and run:

a2enmod ssl
a2ensite default-ssl
service apache2 reload
Self-signed certificates have their drawbacks - especially when you plan to make your ownCloud server publicly accessible. You might want to consider getting a certificate signed by a commercial signing authority. Check with your domain name registrar or hosting service for good deals on commercial certificates.

Run the Installation Wizard
After restarting Apache, you must complete your installation by running either the Graphical Installation Wizard or on the command line with the occ command. To enable this, temporarily change the ownership on your ownCloud directories to your HTTP user (see Set Strong Directory Permissions to learn how to find your HTTP user):

chown -R www-data:www-data /var/www/owncloud/
Admins of SELinux-enabled distributions may need to write new SELinux rules to complete their ownCloud installation; see SELinux.

To use occ see Command Line Installation. To use the graphical Installation Wizard see The Installation Wizard.

Please know that ownCloud’s data directory must be exclusive to ownCloud and not be modified manually by any other process or user.

Set Strong Directory Permissions
After completing the installation, you must immediately set the directory permissions in your ownCloud installation as strictly as possible for stronger security. After you do so, your ownCloud server will be ready to use.

Managing Trusted Domains¶
All URLs used to access your ownCloud server must be whitelisted in your config.php file, under the trusted_domains setting. Users are allowed to log into ownCloud only when they point their browsers to a URL that is listed in the trusted_domains setting.

This setting is important when changing or moving to a new domain name. You may use IP addresses and domain names.

A typical configuration looks like this:

'trusted_domains' => [
   0 => 'localhost',
   1 => 'server1.example.com',
   2 => '192.168.1.50',
],
The loopback address, 127.0.0.1, is automatically whitelisted, so as long as you have access to the physical server you can always log in. In the event that a load-balancer is in place, there will be no issues as long as it sends the correct X-Forwarded-Host header.

For further information on improving the quality of your ownCloud installation, please see the Configuration Notes & Tips guide.    

### Enabling Users to Connect to ownCloud Servers Using IP addresses and port 8080

### Adding user accounts
Follow these steps to add new users:
1. Navigate to the User management page of your ownCloud Web UI.
2. Enter the new user’s Login Name and their initial Password.
3. Optionally, assign Groups memberships.
4. Click **Create**.
 
## For Users
### Connecting to ownCloud servers using desktop clients
### Connecting to ownCloud servers using mobile clients

## Troubleshooting
If you are having problems, see [General Troubleshooting](https://doc.owncloud.org/server/10.0/admin_manual/issues/general_troubleshooting.html) and the [Owncloud Forums](https://central.owncloud.org/).
