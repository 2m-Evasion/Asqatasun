# Asqatasun installation

This page describes the steps to follow to install Asqatasun since Version 3.0.0 
(and further) from the binary files or from the sources. The software has been 
tested on 14.04 LTS (Trusty Tahr).

## Docker

If you want to quickly test Asqatasun, you may use the [Asqatasun Docker Image](../Docker/), 
or you can follow the following full instructions.

## Download Asqatasun tarball and extract content

Retrieve the [lastest version of Asqatasun](http://download.asqatasun.org/asqatasun-latest.tar.gz) and extract it on your file system. 

```sh
wget http://download.asqatasun.org/asqatasun-latest.tar.gz
tar xzf asqatasun-latest.tar.gz
cd asqatasun*
```

## Install and configure pre-requisites


To install and setup all pre-requisites, you can run the script:

```sh
pre-requisites.sh
```

A few noteworthy information:

* This script is intended to be used on a freshly installed Ubuntu 14.04 (no Mysql, 
no Tomcat already installed).
* You can use the default values or adjust them to suite your needs (directly edit the file).
* This script must be run as root
* Pre-requisites are important, and **each detail is important** (e.g. Mysql specific 
configuration, or Tomcat user configuration, or Firefox version requirement...), so please
do care about it :)

If you have a Tomcat or Mysql already installed, or if you don't feel comfortable
with running a script as root, you can review it or follow the 
[pre-requisites **manual** instructions](pre-requisites.md)

## Create the database

Create an empty schema and a asqatasun user. Grant this asqatasun user permissions 
to create, update and delete objects for this schema. The charset of the database 
has to be set to "UTF-8".

```mysql
GRANT USAGE ON * . * TO '$AsqatasunUser'@'localhost' IDENTIFIED BY '$AsqatasunPassword';
CREATE DATABASE IF NOT EXISTS `$AsqatasunDatabase` CHARACTER SET utf8;
GRANT ALL PRIVILEGES ON `$AsqatasunDatabase` . * TO '$AsqatasunUser'@'localhost';
FLUSH PRIVILEGES;
```

where $AsqatasunUser is the asqatasun user, $AsqatasunPassword is the asqatasun user password, and $AsqatasunDatabase is the asqatasun database.

## Execute the installation script

```sh
sudo ./install.sh --database-user <Asqatasunu_dababase_user> \
                  --database-passwd <Asqatasun_dababase_password> \
                  --database-db <Asqatasun_dababase_dbname> \ 
                  --database-host <Asqatasun_dababase_hostname> \ 
                  --asqatasun-url <Asqatasun_webapp_url> \
                  --tomcat-webapps <tomcat_webapps_directory> \
                  --tomcat-user <tomcat_unix_user> \
                  --asqa-admin-email <Asqatasun_admin_email> \
                  --asqa-admin-passwd <Asqatasun_admin_password> \
                  --firefox-esr-binary-path <path_to_Firefox_ESR_binary> \
                  --display-port <Xorg_display_port>
```

### Script options description


* **--database-user** : Mysql user for Asqatasun
* **--database-passwd** : Password of the user specified by --database-user. if this user already exists, please ensure you give its correct password. If not, the user will be automatically created.
* **--database-db** : Database for Asqatasun
* **--asqatasun-url** : URL where asqatasun will be deployed (e.g. http://localhost:8080/asqatasun)
* **--tomcat-webapps** : Tomcat webapps directory (e.g. /var/lib/tomcat7/webapps)
* **--tomcat-user** : Unix user name for the tomcat service (e.g. tomcat7)
* **--asqa-admin-email** : Email of the Asqatasun admin user (by the way, it's you !)
* **--asqa-admin-passwd** : The asqatasun application admin password
* **--firefox-esr-binary-path** : Path to Firefox-ESR binary (e.g. /opt/firefox/firefox) you installed in Asqatasun pre-requisites.
* **--display-port** : Xorg display port (e.g. ":99"). For debug purpose, you may want asqatasun to display firefox instances in the current X session (for desktop). In this case, use ":0" as value and execute the "xhost +" in a terminal to authorize the process (owned by tomcat) to use the X server.

### Example of install-script invocation

```sh
sudo ./install.sh --database-user asqatasun \
                  --database-passwd $MyTGPassWord \
                  --database-db asqatasun \
                  --asqatasun-url http://localhost:8080/asqatasun/ \
                  --tomcat-webapps /var/lib/tomcat7/webapps/ \
                  --tomcat-user tomcat7 \
                  --asqa-admin-email me@email.com \
                  --asqa-admin-passwd toto42 \
                  --firefox-esr-binary-path /opt/firefox/firefox \
                  --display_port :99
```
