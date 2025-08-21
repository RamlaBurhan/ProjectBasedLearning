# Deployment of a LAMP Stack on an AWS EC2 Instance

## Description:
This project provides a step-by-step guide to installing configuring, and integrating each component of the LAMP stack (Linux, Apache, MySQL, and PHP) on an AWS ECS instance. The goal is to set up a fully functional web server environment for deploying applications.

---

## Prequisites
- AWS Account
- An EC2 Keypair

---
## Below are key steps for deploying a LAMP Stack on an AWS EC2 Instance

## Step 1) Launch EC2 Instance

1. Check security groups
Allow the following inbound rules:
- Port 80/TCP
- Port 443/TCP
- Port 22/TCP

---

## Step 2) Install and Configure Apache2

Apache is installed to serve your content

1. update all software and install apache2
```Bash
sudo apt update
sudo apt install apache2
```



2. See if Apache2 is running

```Bash
sudo apt systemctl status apache2
```

3. Use the curl command to verify that Apache is running.

The server is now running and can be tested from the local machine using the curl command to send a request to localhost.

```Bash
curl http://localhost:80
```

4. We just made apache web service respond to curl command with some payload



---

## Step 2) Install and configure MySQL

Mysql is used to store and manage your data

1. Install MySQL

```Bash
Sudo apt mysql-server
```

2. Login to MySQL console
Once the installation finished, log in to MySQL console by typing

```Bash
Sudo mysql
```
This will allow you to navigate the console as the adminstrator user root

3. Set password for the root user

```Bash
ALTER USER 'roo'@'localhost' IDENTIFIED WITH mysql_native_password BY'*****';
```

4. Exit MySQL shell

```Bash
mysql> exit
```

5. Configured the Validate Password plugin

```Bash
sudo mysql_secure_installation
```

This goes through a series of steps to validate your password. It will prompt you to:
- change root password
- remove anonymous users 
- Remove the test database
- Disable remote root logins

6. Test login

Once you're finished with the validation process, test if you're able to login

```Bash
Sudo mysql -p
```
It will prompt you for the password of the root user



once you've checked, your MySQL server is now installed and secured

---

## Step 3) Install and configure PHP

PHP processes the code needed to display dynamic content to the end user.

1. In addition to PHP, which will install all dependencies, you'll need:
- libapache2-mod-ph: to enable Apache to handle PHP files. 
- php-mysql: A PHP module that allows PHP to communicate with MySQL-based databases.

```BASH
 sudo apt install php libapache2-mod-php php-msql
 ```
2. Confirm versiom

You can run the following command to confirm your PHP version

```Bash
php -v
```



## You have succesfully installed your lampstack and it is fully operational

To test your setup with a PHP script, you can setup a Apache Virtual Host to hold your website's files and folders. Virtual hosts allow you to have multiple website located on a single machine. 

---

## Step 4) Create a virtual host for your website usimg Apache

1. Create a directory for your website and change ownership of directory

```Bash 
sudo mkdit /var/www/projectlamp
sudo chown -R $user:$USER /var/www/projectlamp
```

2) create a new configuration file in Apache's sites-available directory

```Bash
sudo vi /etc/apache2/sites-available/projectlamp.conf
```

Click i and insert the following text below


To save and close file, pres escape then :wq

New file has been created


3. Enable the new virtual host

```Bash
sudo a2ensite projectlamp
```

4. Disable Apache's default website 

```Bash
sudo a2dissite 000-default
```

5. Check for errors in config file

```Bash
sudo apache2ctl configtest
```

6. Reload Apache for changes to take intp effect

```Bash
sudo systemctl reload apache2
```

7. Open browser

if your browser opens, it means your Apache virtual host is working


--- 

## Step 5) Enable PHP on the website

1. With the default DirectoryIndex settings on Apache, a file name index.html will always take precedence ove and index.php file.

if you want to change the behaviour you'll need to edit  the /etc/apache2/mods-enabled/dir.conf file and change the order in which the indec.php file is listed with then DirectoryIndex directive

```Bash
sudo vim /etc/apache2/mods-enabled/dir.conf
```
current order:


change order:

2. reload Apache so the changes can take effect


3. create a new file and add a valid php code

```Bash
vim /var/www/projectlamp/index.php
```

<?php
phpinfo() ;

4. Check PHP installation has worked

If you can see the following in your browser, the php installation has been successfully installed

- This page provide information about your server fro mthe perspective of PHP. It is usefull for debugging and to ensure that your setting are being applied correctly


5. Remove index.php. file

After checking the relevant information about you PHP server, it is best to remove the file you created as it contains sensitive information about you php environment and your ubuntu server







