title: "CentOS7.0 LAMP"
date: 2015-04-04 14:12:03
tags:
id: 484
categories:
  - 技术工具
---

* Setting the Hostname:
hostnamectl set-hostname plato

** CentOS6
echo “HOSTNAME=plato” &gt;&gt; /etc/sysconfig/network
hostname “plato”

* Update /etc/hosts
12.34.56.78 plato.example.com plato
<span id="more-5"></span>

* Setting the timezone
list timezone
timedatectl list-timezones
setting
timedatectl set-timezone Asia/Shanghai

<!--more-->* Checking the Time
$ date

* Installing software updates
yum update

* Install and Configure Apchae Web Server
$sudo yum install httpd
** backup configure
$ cp /etc/httpd/conf/httpd.conf ~/httpd.conf.backup
** enable apache and boot run first time
$ sudo /bin/systemctl enable httpd.service
$ sudo /bin/systemctl start httpd.service
$ systemctl restart httpd.service
* install and configure mariaDB Database server
systemctl start vsftpd.service

$ sudo yum install mariadb-server
** Install
1\. sudo yum install mariadb-server
2\. sudo /bin/systemctl enable mariadb.service
3\. sudo /bin/systemctl start mariadb.service
4\. cp /etc/my.cnf ~/my.cnf.backup
** Configure MariaDB and Set Up MariaDB databases
1\. mysql_secure_installation
2\. mysql -u root -p
3\. create new database
create database example_database_name;
grant all on example_database_name.* to ‘example_user’@’localhost’ identified by ‘example_password';
* Install and configuring php
1\. sudo yum install php php-pear
2\. emacs /etc/php.ini
error_log = /var/log/php/error.log
3\. sudo mkdir /var/log/php
sudo chown apache /var/log/php
4\. sudo yum install php-mysql
5\. sudo /bin/systemctl reload httpd
yum -y install php-gd
yum -y install php-mbstring

* add system user and configure permissions
– adduser username
– passwd username #change password
– edite permissions
$visudo
## Allow root to run any commands anywhere
root ALL=(ALL) ALL
user ALL=(ALL) ALL