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

1. Check security groups.
Allow the following inbound rules:
- Port 80/TCP
- Port 443/TCP
- Port 22/TCP
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture1.png)
---

## Step 2) Install and Configure Apache2

Apache2 is installed to serve your content

1. Update all software and install apache2
```Bash
sudo apt update
sudo apt install apache2
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture3.png)

2. See if Apache2 is running

```Bash
sudo apt systemctl status apache2
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture2.png)

**3. Use the curl command to verify that Apache is running.**

The server is now running and can be tested from the local machine using the curl command to send a request to localhost.

```Bash
curl http://localhost:80
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture10.png)

Apache2 web service responds to the curl command with some payload

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture11.png)

4. Type into web browser 

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture9.png)

Apache had been successfully installed

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
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY'*****';
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture8.png)

4. Exit MySQL shell

```Bash
mysql> exit
```

5. Configured the Validate Password plugin

```Bash
sudo mysql_secure_installation
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture7.png)

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

## Step 4) Create a virtual host for your website usimg Apache2

1. Create a directory for your website and change ownership of directory

```Bash 
sudo mkdir /var/www/projectlamp
sudo chown -R $user:$USER /var/www/projectlamp
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture19.png)

2) Create a new configuration file in Apache's sites-available directory

```Bash
sudo vi /etc/apache2/sites-available/projectlamp.conf
```
Click i and insert the following text below
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture13.png)

To save and close file, pres escape then :wq

You can type ls to show new file thathas been created

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture17.png)

3. Disable Apache's default website 

```Bash
sudo a2dissite 000-default
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture14.png)

4. Enable the new virtual host 

```Bash
sudo a2ensite projectlamp
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture16.png)


5. Check for errors in config file

```Bash
sudo apache2ctl configtest
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture12.png)

6. Reload Apache for changes to take into effect

```Bash
sudo systemctl reload apache2
```

7. Test virtual host works
- Create an index.html in the web root var/www/project lamp

Type into the commandline:
```
sudo sh -c "echo 'Hello LAMP from hostname' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token/" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token/" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

7. Open browser

If your browser opens, it means your Apache virtual host is working

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/641bc8e2834cf4e6a89696bf8eda74e2d9837ac9/LAMP_Stack/Images/Picture25.png)

--- 

## Step 5) Enable PHP on the website

1. With the default DirectoryIndex settings on Apache, a file name index.html will always take precedence ove and index.php file.

If you want to change the behaviour, you'll need to edit  the /etc/apache2/mods-enabled/dir.conf file and change the order in which the indec.php file is listed with then DirectoryIndex directive

```Bash
sudo vim /etc/apache2/mods-enabled/dir.conf
```
current order:

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/551ca6d895bb8c4c302ea301661261f666a047c3/LAMP_Stack/Images/Picture23.png)

change order:

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/551ca6d895bb8c4c302ea301661261f666a047c3/LAMP_Stack/Images/Picture22.png)

2. reload Apache so the changes can take effect

```Bash
sudo systemctl reload apache2
```

3. create a new index.php file

```Bash
vim /var/www/projectlamp/index.php
```
- Add a valid PHP code
<?php
phpinfo() ;

4. Check PHP installation has worked

If you can see the following in your browser, the php installation has been successfully installed
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/551ca6d895bb8c4c302ea301661261f666a047c3/LAMP_Stack/Images/Picture21.png)

- This page provide information about your server from the perspective of PHP. It is usefull for debugging and to ensure that your setting are being applied correctly

5. Remove index.php file

After checking the relevant information about you PHP server, it is best to remove the file you created as it contains sensitive information about you php environment and your ubuntu server

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/e289665294bc6603898c9cf9c0a45246cf5d3880/LAMP_Stack/Images/Picture20.png) 






