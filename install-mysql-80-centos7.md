

[Source](https://www.if-not-true-then-false.com/2010/install-mysql-on-fedora-centos-red-hat-rhel/ "Permalink to Install MySQL 8.0/5.7 on Fedora 29/28, CentOS/RHEL 7.5/6.10 – If Not True Then False")

# Install MySQL 8.0/5.7 on Fedora 29/28, CentOS/RHEL 7.5/6.10 – If Not True Then False

![MySQL Logo][1]

[Are you looking MariaDB 10.3/10.2 Install guide?][2]

MySQL is a relational database management system (RDBMS) that runs as a server providing multi-user access to a number of databases. This is guide, **howto install or upgrade MySQL Community Server latest version 8.0 (8.0.13)/5.7 (5.7.24) on Fedora 29/28/27, CentOS 7.5/6.10 and Red Hat (RHEL) 7.5/6.10**. This guide works of course with **Oracle Linux** and **Scientific Linux** too.

**Note: If you are upgrading MySQL (from earlier version), then make sure that you backup (dump and copy) your database and configs. And remember run **mysql_upgrade** command.**

## Install MySQL Database 8.0.13/5.7.24 on Fedora 29/28/27, CentOS 7.5/6.10, Red Hat (RHEL) 7.5/6.10

### 1\. Change root user
    
    
    su -
    ## OR ##
    sudo -i
    

### 2\. Install MySQL YUM repository

#### Fedora
    
    
    ## Fedora 29 ##
    dnf install https://dev.mysql.com/get/mysql80-community-release-fc29-1.noarch.rpm
    
    ## Fedora 28 ##
    dnf install https://dev.mysql.com/get/mysql80-community-release-fc28-1.noarch.rpm
    
    ## Fedora 27 ##
    dnf install https://dev.mysql.com/get/mysql80-community-release-fc27-1.noarch.rpm
    

#### CentOS and Red Hat (RHEL)
    
    
    ## CentOS 7 and Red Hat (RHEL) 7 ##
    yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
    
    ## CentOS 6 and Red Hat (RHEL) 6 ##
    yum localinstall https://dev.mysql.com/get/mysql80-community-release-el6-1.noarch.rpm
    

### 3a. Update or Install MySQL 8.0.13

#### Fedora 29/28/27
    
    
    dnf install mysql-community-server
    

#### CentOS 7.5/6.10 and Red Hat (RHEL) 7.5/6.10
    
    
    yum install mysql-community-server
    

### 3b. Update or Install MySQL 5.7.24

#### Fedora 29/28/27
    
    
    dnf --disablerepo=mysql80-community --enablerepo=mysql57-community install mysql-community-server
    

#### CentOS 7.5/6.10 and Red Hat (RHEL) 7.5/6.10
    
    
    yum --disablerepo=mysql80-community --enablerepo=mysql57-community install mysql-community-server
    

### 4\. Start MySQL server and autostart MySQL on boot

#### Fedora 29/28/27 and CentOS 7.5 and Red Hat (RHEL) 7.5
    
    
    systemctl start mysqld.service ## use restart after update
    
    systemctl enable mysqld.service
    

#### CentOS 6.10 and Red Hat (RHEL) 6.10
    
    
    /etc/init.d/mysql start ## use restart after update
    ## OR ##
    service mysql start ## use restart after update
    
    chkconfig --levels 235 mysqld on
    

### 5\. Get Your Generated Random root Password
    
    
    grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |tail -1
    

Example Output:
    
    
    2015-11-20T21:11:44.229891Z 1 [Note] A temporary password is generated for root@localhost: -et)QoL4MLid
    

And root password is: **-et)QoL4MLid**

### 6\. MySQL Secure Installation

* **Change root password**
* **Remove anonymous users**
* **Disallow root login remotely**
* **Remove test database and access to it**
* **Reload privilege tables**

#### Start MySQL Secure Installation with following command
    
    
    /usr/bin/mysql_secure_installation
    

Output:
    
    
    Securing the MySQL server deployment.
    
    Enter password for user root: 
    
    The existing password for the user account root has expired. Please set a new password.
    
    New password: 
    
    Re-enter new password: 
    
    VALIDATE PASSWORD PLUGIN can be used to test passwords
    and improve security. It checks the strength of password
    and allows the users to set only those passwords which are
    secure enough. Would you like to setup VALIDATE PASSWORD plugin?
    
    Press y|Y for Yes, any other key for No: y
    
    There are three levels of password validation policy:
    
    LOW    Length >= 8
    MEDIUM Length >= 8, numeric, mixed case, and special characters
    STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file
    
    Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
    Using existing password for root.
    
    Estimated strength of the password: 100 
    Change the password for root ? ((Press y|Y for Yes, any other key for No) : y
    
    New password: 
    
    Re-enter new password: 
    
    Estimated strength of the password: 50 
    Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
    By default, a MySQL installation has an anonymous user,
    allowing anyone to log into MySQL without having to have
    a user account created for them. This is intended only for
    testing, and to make the installation go a bit smoother.
    You should remove them before moving into a production
    environment.
    
    Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
    Success.
    
    
    Normally, root should only be allowed to connect from
    'localhost'. This ensures that someone cannot guess at
    the root password from the network.
    
    Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
    Success.
    
    By default, MySQL comes with a database named 'test' that
    anyone can access. This is also intended only for testing,
    and should be removed before moving into a production
    environment.
    
    
    Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
     - Dropping test database...
    Success.
    
     - Removing privileges on test database...
    Success.
    
    Reloading the privilege tables will ensure that all changes
    made so far will take effect immediately.
    
    Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
    Success.
    
    All done! 
    

**Note: If you don't want some reason, do a "MySQL Secure Installation" then at least it's very important to change the root user's password**
    
    
    mysqladmin -u root password [your_password_here]
    
    ## Example ##
    mysqladmin -u root password myownsecrectpass
    

### 7\. Connect to MySQL database (localhost) with password
    
    
    mysql -u root -p
    
    ## OR ##
    mysql -h localhost -u root -p
    

### 8\. Create Database, Create MySQL User and Enable Remote Connections to MySQL Database

**This example uses following parameters:**

* DB_NAME = webdb
* USER_NAME = webdb_user
* REMOTE_IP = 10.0.15.25
* PASSWORD = password123
* PERMISSIONS = ALL
    
    
    ## CREATE DATABASE ##
    mysql> CREATE DATABASE webdb;
    
    ## CREATE USER ##
    mysql> CREATE USER 'webdb_user'@'10.0.15.25' IDENTIFIED BY 'password123';
    
    ## GRANT PERMISSIONS ##
    mysql> GRANT ALL ON webdb.* TO 'webdb_user'@'10.0.15.25';
    
    ##  FLUSH PRIVILEGES, Tell the server to reload the grant tables  ##
    mysql> FLUSH PRIVILEGES;
    

## Enable Remote Connection to MariaDB Server –> Open MySQL Port (3306) on Iptables Firewall (as root user again)

### 1\. Fedora 29/28/27 and CentOS/Red Hat (RHEL) 7.5

#### 1.1 Add New Rule to Firewalld
    
    
    firewall-cmd --permanent --zone=public --add-service=mysql
    
    ## OR ##
    
    firewall-cmd --permanent --zone=public --add-port=3306/tcp
    

#### 1.2 Restart firewalld.service
    
    
    systemctl restart firewalld.service
    

### 2\. CentOS/Red Hat (RHEL) 6.10

#### 2.1 Edit /etc/sysconfig/iptables file:
    
    
    nano -w /etc/sysconfig/iptables

#### 2.2 Add following INPUT rule:
    
    
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

### 2.3 Restart Iptables Firewall:
    
    
    service iptables restart
    ## OR ##
    /etc/init.d/iptables restart
    

### 3\. Test remote connection
    
    
    mysql -h 10.0.15.25 -u myusername -p
    

[1]: https://media.if-not-true-then-false.com/2010/07/mysql-logo.png "MySQL Logo"
[2]: https://www.if-not-true-then-false.com/2013/install-mariadb-on-fedora-centos-rhel/

  

