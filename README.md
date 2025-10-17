# Installation and Configuration of Zabbix 7.0 on Ubuntu 24.04

Purpose

This SOP provides step-by-step instructions to install and configure Zabbix 7.0 Monitoring Server with MySQL and Apache on Ubuntu 24.04.

Scope

Applicable for Infrastructure and IT administrators responsible for setting up Zabbix monitoring solutions.

Prerequisites

Ubuntu 24.04 server with root or sudo access.

Internet connectivity to download packages.

Basic Linux administration knowledge.

Procedure
1. Update and Install Required Packages

Run the following commands to update the system and install dependencies:

sudo apt update && sudo apt upgrade -y
sudo apt install apache2 -y
sudo apt install php php-{cgi,common,mbstring,net-socket,gd,xml-util,mysql,bcmath,imap,snmp} -y
sudo apt install libapache2-mod-php -y

2. Install and Configure Zabbix
a. Switch to Root User
sudo -s

b. Add Zabbix Repository
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update

c. Install Zabbix Components
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y

3. Install and Configure MySQL Database
a. Install MySQL Server
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql

b. Secure MySQL Installation
sudo mysql_secure_installation


Recommended responses:

n,y,y,y,y


Verify service status:

sudo systemctl status mysql

4. Create Initial Database for Zabbix
a. Login to MySQL
mysql -uroot -p

b. Run SQL Commands
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'Secure@1234';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;

c. Import Zabbix Database Schema
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

d. Disable Function Creators Option
mysql -uroot -p

set global log_bin_trust_function_creators = 0;
quit;

5. Configure Zabbix Server

Edit configuration file:

sudo nano /etc/zabbix/zabbix_server.conf


Update database password:

DBPassword=Secure@1234

6. Start and Enable Zabbix Services
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

Verification

Open a web browser and navigate to:

http://<your_server_ip_or_hostname>/zabbix


Follow the web installer wizard to complete frontend setup.

Confirm that the Zabbix server and agent services are running:

systemctl status zabbix-server zabbix-agent apache2


✅ Zabbix 7.0 is now installed and configured successfully.
