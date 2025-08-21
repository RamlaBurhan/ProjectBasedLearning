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

1. update all software and install apache2
```Bash
sudo apt update
sudo apt install apache2
```



2. See if Apache2 is running

```Bash
sudo apt systemctl apache2
```

3. Use the curl command to verify that Apache is running.

The server is now running and can be tested from the local machine using the curl command to send a request to localhost.

```Bash
curl http://localhost:80
```

4. We just made apache web service respond to curl command with some payload



---

## Step 2) Install and configure MySQL

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
