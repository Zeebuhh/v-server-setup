# V-Server Setup Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Initial Setup](#initial-setup)
   - [Generate and Save SSH Keys](#generate-and-save-ssh-keys)
   - [Log Into the Server](#log-into-the-server)
3. [Secure the Server](#secure-the-server)
   - [Add SSH Key to the Server](#add-ssh-key-to-the-server)
   - [Disable Password Login](#disable-password-login)
4. [Install and Configure Nginx](#install-and-configure-nginx)
   - [Install Nginx](#install-nginx)
   - [Create a Custom Web Page](#create-a-custom-web-page)
5. [Set Up Git](#set-up-git)
6. [Conclusion](#conclusion)

---

## Introduction

This guide outlines the steps to set up a virtual server (V-Server) with enhanced security, Nginx as a web server, and Git for version control. Follow each section in sequence to ensure a successful setup.

Repository: [GitHub Repository](https://github.com/Zeebuhh/v-server-setup)

Server IP Address: `159.69.20.89`

---

## Initial Setup

### Generate and Save SSH Keys

1. Generate SSH keys on your local machine for secure server access:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
2. Save the generated keys in a secure location.

### Log Into the Server

1. Use the provided IP address, username, and password to log into the server:
   ```bash
   ssh user@hostipaddress
   ```
   Password: `userpassword`
2. Verify the connection by logging in successfully, then log out:
   ```bash
   logout
   ```

---

## Secure the Server

### Add SSH Key to the Server

1. Copy your public SSH key to the server:
   ```bash
   ssh-copy-id -i ~/.ssh/id_ed25519.pub user@hostipaddress
   ```
   Enter the server password when prompted.
2. Confirm the key has been added successfully:
   ```bash
   ssh -i ~/.ssh/id_ed25519 user@hostipaddress
   ```
   Check for the presence of your key:
   ```bash
   ls -al ~/.ssh
   cat ~/.ssh/authorized_keys
   ```

### Disable Password Login

1. Edit the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Locate and update the following line:
   ```bash
   PasswordAuthentication no
   ```
   Remove the `#` if it exists at the start of the line.
3. Save changes (Ctrl+O, Enter) and close the file (Ctrl+X).
4. Restart the SSH service:
   ```bash
   sudo systemctl restart ssh.service
   ```
5. Test the configuration by logging in using your SSH key and verifying that password login is disabled:
   ```bash
   ssh -i ~/.ssh/id_ed25519 user@hostipaddress
   ssh -o PubkeyAuthentication=no user@hostipaddress
   ```
   The latter command should return a "Permission denied" message.

---

## Install and Configure Nginx

### Install Nginx

1. Log into the server and update the package repositories:
   ```bash
   sudo apt update
   ```
2. Install Nginx:
   ```bash
   sudo apt install nginx -y
   ```
3. Verify the installation:
   ```bash
   systemctl status nginx.service
   ```
   Look for `Active: active (running)` in the output.
4. Access the server's IP address in a browser to view the default Nginx page.

### Create a Custom Web Page

1. Create a directory and an HTML file for the custom page:
   ```bash
   sudo mkdir /var/www/alternatives
   sudo touch /var/www/alternatives/alternate-index.html
   ```
2. Verify the creation:
   ```bash
   ls /var/www/
   ls /var/www/alternatives
   ```
3. Configure Nginx to serve the custom page:

   ```bash
   sudo nano /etc/nginx/sites-enabled/alternatives
   ```

   Add the following configuration:

   ```nginx
   server {
       listen 8081;
       root /var/www/alternatives;
       index alternate-index.html;

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```

4. Save the file and restart Nginx:
   ```bash
   sudo service nginx restart
   systemctl status nginx.service
   ```
5. Edit the custom HTML file to include your content:
   ```bash
   sudo nano /var/www/alternatives/alternate-index.html
   ```
6. Test the custom page by visiting `http://hostipaddress:8081`.

---

## Set Up Git

1. Install Git on the server:
   ```bash
   sudo apt install git -y
   ```
2. Generate a new SSH key for the server to connect to GitHub:
   ```bash
   ssh-keygen -t ed25519 -C "github key"
   ```
3. Display and copy the public key:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   Add this key to your GitHub account under **Settings > SSH Keys**.
4. Configure Git user details:
   ```bash
   git config --global user.name "your_username"
   git config --global user.email "your_email@example.com"
   ```
5. Clone your repository:
   ```bash
   git clone git@github.com:YourName/v-server-setup.git
   ```

---

## Conclusion

Congratulations! Your V-Server is now set up with a secure SSH connection, a functional web server using Nginx, and Git integration.
