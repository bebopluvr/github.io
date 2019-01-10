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
#### Enable and Install Required Extensions
1. Enable the RHEL Server 7 repository

     `subscription-manager repos --enable rhel-server-rhscl-7-eus-rpms`

2. Install the required packages

      `sudo yum install httpd mariadb-server php72 php72-php \
       php72-php-gd php72-php-mbstring test php72-php-mysqlnd`
    
#### Install ownCloud
Follow these steps:
1. Download the archive of the laownCloud version:
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
      `cp -r owncloud /path/to/webserver/document-root
      
   where `/path/to/webserver/document-root` is replaced by the document root of your Web server:
      `cp -r owncloud /var/www`
      
   On other HTTP servers, we recommend installing ownCloud outside of the document root.

#### Configure Apache Web Server
The Apache HTTP Server provides an open-source HTTP server with the current HTTP standards.
In Red Hat Enterprise Linux, the httpd package provides the Apache HTTP Server. Run the `rpm -q httpd` command to see if the httpd package is installed. If it is not installed, run the following command as the root user to install it:
`yum install httpd`

#### Run the Installation Wizard
After restarting Apache, you must complete your installation by running either the Graphical Installation Wizard or on the command line with the occ command. To enable this, temporarily change the ownership on your ownCloud directories to your HTTP user (see Set Strong Directory Permissions to learn how to find your HTTP user):

chown -R www-data:www-data /var/www/owncloud/
Admins of SELinux-enabled distributions may need to write new SELinux rules to complete their ownCloud installation; see SELinux.

To use occ see Command Line Installation. To use the graphical Installation Wizard see The Installation Wizard.

Please know that ownCloud’s data directory must be exclusive to ownCloud and not be modified manually by any other process or user.

#### Set Strong Directory Permissions
After completing the installation, you must immediately set the directory permissions in your ownCloud installation as strictly as possible for stronger security. After you do so, your ownCloud server will be ready to use.

####  Trusted Domains¶
You must whitelist all URLs used to access your ownCloud server in your config.php file, under the trusted_domains setting. Users are allowed to log into ownCloud only when they point their browsers to a URL that is listed in the trusted_domains setting. This setting is important when changing or moving to a new domain name. You may use IP addresses and domain names.

A typical configuration looks like this:

'trusted_domains' => [

   0 => 'localhost',
   
   1 => 'server1.example.com',
   
   2 => '192.168.1.50',
],

The loopback address, 127.0.0.1, is automatically whitelisted, so as long as you have access to the physical server you can always log in. In the event that a load-balancer is in place, there are no issues as long as it sends the correct X-Forwarded-Host header.  

### Enabling Users to Connect to ownCloud Servers Using IP addresses and port 8080

### Adding user accounts
Follow these steps to add new users:
1. Navigate to the User management page of your ownCloud Web UI.
2. Enter the new user’s Login Name and their initial Password.
3. Optionally, assign Groups memberships.
4. Click **Create**.
Login names may contain letters (a-z, A-Z), numbers (0-9), dashes (-), underscores (_ \) periods (.) and at signs (@). After creating the user, you may fill in their Full Name if it is different than the login name, or leave it for the user to complete.

If you have checked **Send email to new user** in the control panel on the lower left sidebar, you may also enter the new user’s email address, and ownCloud will automatically send them a notification with their new login information. You may edit this email using the email template editor on your Admin page.
 
## For Users.
### Connecting to ownCloud servers using desktop clients
### Connecting to ownCloud servers using mobile clients

## Troubleshooting
If you are having problems, see [General Troubleshooting](https://doc.owncloud.org/server/10.0/admin_manual/issues/general_troubleshooting.html) and the [Owncloud Forums](https://central.owncloud.org/).
