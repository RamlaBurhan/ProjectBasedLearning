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

