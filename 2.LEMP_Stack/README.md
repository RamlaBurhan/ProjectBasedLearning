# Deployment of a LEMP Stack on an AWS EC2 Instance

## Description
This project provides a step-bystep guide to installing and configuring and integrating each component of the LEMP stack (Linux, Nginx, Mysql and PHP) ON an AWS EC2 instance. The goal is to set up a fully functional web server environment for deploying applications

---

## Prerequisites

- AWS Account
- An EC2 Keypair

---

## Below are key steps for deploying a LEMP Stack on an AWS EC2 Instance

## step 1) Launch EC2 Instance

1. Check security groups.
Allow the following inbound rules:
- Port 80/TCP
- Port 443/TCP
- Port 22/TCP

---

## Step 2) Install and Configure NGINX

NGINX is installed to serve your content

1. Update all software and install NGINX

``` Bash
sudo apt update
sudo apt install nginx
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%202.png)

2. See if NGINX is running

```Bash
sudo apt systemctl status NGINX
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%203.png)

3. Use the curl command to verify that NGINX is running

The server is now running and can be tested from the local machine using the curl command to send a request to localhost

```Bash
curl http://localhost:80
```

NGINX web service responds to the curl command with some payload

image

4. Test if Nginx server can respond to requests from the Internet.

---

## Step 3) Install and configure MySQL

MySQL is used to store and manage your data

1. Install MySQL

```Bash
 sudo apt install mysql-server
 ```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%205.png)

 2. Login to MySQL console
 Once the installation has finished, log in to MySQL

 ```Bash
 sudo mysql
 ```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%206.png)

 This will allow you to navigate the console as the adminstrator user root

3. Set password for the root user

```Bash
ALTERUSER'root'@'localhost' IDENTIFIED WITH mysql_native_password BY'******;
```

4. Exit MySQL shell

```Bash
mysql> exit
```

5. Configured the Validate Password plugin

```Bash
sudo mysql_secure_installation
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%207.png)

This goes through a series of steps to validate your password. It will prompt you to:
- change root password
- remove anonymous users 
- Remove the test database
- Disable remote root logins

6. Test login

Once you're finished with the validation process, test if you're able to login.

```Bash
Sudo mysql -p
```
It will prompt you for the password of the root user
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%209.png)

once you've checked, your MySQL server is now installed and secured

---

## Step 4) Install and configure PHP

PHP processes the code needed to display dynamic content to the end user.


```BASH
 sudo apt install php 
 ```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2010.png)

Then install:

```Bash
sudo apt install php-fpm php-mysql
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2011.png)

---

## step 5) Configuring Nginx to use PHP Processor

1. Create root web director for my domain

```Bash
sudo mkdir /var/www/projectLEMP
```

2. Assign ownership of the directory with the $USER environment variable

```Bash
sudo chown -R $USER:$USER /var/www/projectLEMP
```

3. Create a new configuration file in Nginx's sites-available directory

```Bash
sudo nano /etc/nginx/sites-available/projectLEMP
```

Paste the following in the file above:

```Bash
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```
6. - Activate the configuration  by linking to the config file from Nginx's sites-enabled directory:
   - Check that Nginx syntax has no errors

```Bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
sudo nginx -t
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/be0848fd62d401651cfbbc4240bdb1b7e1892b5e/2.LEMP_Stack/Images/Image%2013.png)

8. - Disable default Nginx host that is configured to listen on port 80
   -  Reload Nginx to apply changes
```Bash
sudo unlink /etc/nginx/sites-enabled/default
sudo systemctl reload nginx
```
![image alt] (https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2013.png)

9. Create an index.html in /var/www/projecLEMP directory to test new server block

```Bash
sudo echo 'Hello LEMP from hostname' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

11. Open website URL using IP address

```Bash
http://<Public-IP-Address>:80
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2014.png)

## Step 6) Testing PHP with Nginx

validate that Nginx can correctly hand .php files off to your PHP processor

1. Create a test info.php file

```Bash
vi /var/www/projectLEMP/info.php
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2015.png)

Paste the following in the document:
<?php
phpinfo();>

2. Access page via web browser

3. Once checked, remove the info.php file

```Bash
Sudo rm /var/www/54.83.127.210/info.php
```

## Step 7) Retrieving data from MySQL database with PHP

I created a test database (DB) with a simple ''To do list'' and configured access to it, so the Nginx website would be able to query data from the DB and display it.]

1. Connect to MySQL using the root account

```Bash
Sudo mysql
```

2. Create a new database
```Bash
mysql > CREATE DATABASE 'example_database';
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2018.png)

3. Create an user and grant full privileges on the database
```Bash
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY *********;
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2019.png)

4. Exit
```Bash
mysql> exit
```

6. Test if the new user has proper permission
```Bash
mysql -u example_user -p
```
- Confirm you have access to example_database

```Bash
mysql> SHOW DATABASES;
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2020.png)

- Create a To-do list table

```Bash
CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
);
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2021.png)

- Insert a few rows of content in the To-do list table

![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2022.png)

- To confirm that the data was successfully saved to your table, run:
```Bash
mysql>  SELECT * FROM example_database.todo_list;
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2023.png)

- Create a PHP script that will connect to MySQL and query for your content
```Bash
$ nano /var/www/projectLEMP/todo_list.php
```

Copy this into your todo_list.php scripts
```Bash
<?php
$user = "example_user";
$password = ******;
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: ". $e->getMessage() . "<br/>";
    die();
}
```

- You can access this page in your web browser 
```Bash
http://54.82.127.210/todo_list.php
```
![image alt](https://github.com/RamlaBurhan/ProjectBasedLearning/blob/69fd5415ba9a56b1b5c49c44f928b30f967061d0/2.LEMP_Stack/Images/Image%2024.png)

This means that your PHP environment is ready to connect and interact with your MySQL server
