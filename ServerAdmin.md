# Server Administration
## Table of Contents

1. [Hosting a Laravel app using the LEMP Stack](#1-lemp-stack)
2. [Hosting a Laravel app using the LAMP Stack](#2-lamp-stack)
3. [Server Hardening Techniques](#3-server-hardening)
4. [Hosting a Flask app](#4-flask)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
- - - - - 

## 1. LEMP Stack
>  ## Prerequisites
- Ubuntu Server
  - It can be obtained from any hosting provider such as [Digital Ocean](https://www.digitalocean.com/), [AWS](https://aws.amazon.com/), [Azure](https://azure.microsoft.com/en-us/), [Google Cloud](https://cloud.google.com/), [Vultr](https://www.vultr.com/), [Linode](https://www.linode.com/), [Interserver](https://www.interserver.net/vps/?id=517869), [Contabo](https://contabo.com/en/vps/?source=affiliate&AID=15022370&PID=100657332&SID=&CJEVENT=0ec48e81839511ee815d212d0a18ba72).
    
- Git repository of a Laravel application
  
- Domain
   - Register and buy a new domain for the Laravel application through [Namecheap](https://www.namecheap.com/).
     
### What is a LEMP Stack?
- It's a combination of four technologies which include Linux, Enginx, MySql and Php. This stack can be used to host web apps built using php frameworks such as Laravel.

### Steps to host the Laravel app
### Login to the Ubuntu Server
- Open the terminal and login as the root in the server:
  ```
   ssh root@ipaddress
  ```
  
- Create a new user:
  
  ```
   sudo adduser user
  ```

- Add the new user in sudo visudo:
  
  ```
   sudo visudo
  ```

  ```sh
   # User privilege specification
  root    ALL=(ALL:ALL) ALL
  ```
  
- Logout and login as the new user:
  
  ```
   ssh newuser@ipaddress
  ```
  `NOTE:` `newuser` is the new user you created and `ipaddress` is the ipaddress of your server. `Avoid running commands using root` for the next steps.
  
### Update the package manager
- Ensure the server is upto date before installing any packages.

   ```
   sudo apt-get update
   ```

### Install Nginx (Enginx)
- Install nginx in your server:
  
  ```
    sudo apt-get install nginx
  ```

- Confirm if it has installed by checking `http://server-ipaddress` on the browser. The nginx welcome page will be displayed.

### Install MySql
- Install the MySql server where the laravel app database will be stored.

  ```
    sudo apt-get install mysql-server
  ```

### Secure MySql
- Secure the database server in production:
  
  ```
    sudo mysql_secure_installation
  ```
  
  Configurations:
  
  ```
  VALIDATE PASSWORD: N
  Change existing password for root: N
  Removing anonymous users: Y
  Disallow root login remotely: Y
  Remove the test database: Y
  Reload the privileges table: Y
  ```

### Install PHP
- Plugins to install:
  - `php-fpm`: Stands for FastCGI Process Manager. Used for processing.
    
  - `php-mysql`: Allows one to query MySQL using Eloquent.
    
  - `php-mbstring`: Laravel requirement.

  ```
    sudo add-apt-repository -y ppa:ondrej/php
    sudo apt-get update
    sudo apt-get install php-fpm php-mysql php-mbstring unzip php-json php-bcmath php-zip php-gd php-tokenizer php-xml
  ```

   `NOTE:` If you get the error `apt add repository command not found`, run `sudo apt-get install software-properties-common`. Afterwards try and install PHP again.

- Confirm the PHP version installed:
  
  ```
     php -v
  ```

### Install Composer
- Composer: PHP dependency manager that will keep track of libraries and dependencies that PHP applications need. Install it:

  ```
    curl -sS https://getcomposer.org/installer | php 
  ```

- Move the downloaded binary to the system directory:

  ```
    sudo mv composer.phar /usr/local/bin/composer
  ```

- Confirm the composer version installed:

  ```
    composer --version
  ```

### Configure PHP
- Install the `nano` editor:

  ```
    sudo apt-get install nano
  ```
  
- Modify `php.ini` file:

  ```
    sudo nano /etc/php/8.2/fpm/php.ini
  ```

  `NOTE:` Replace `8.2` with the php version installed.

  - Press `ctrl` + `w` and search for `cgi.fix_pathinfo=`. Uncomment the line and replace `1` to `0`.
  - Press `ctrl` + `x` and enter `Y` to save the file. Press the enter key to exit nano.

- Restart `php-fpm` for the changes to take place:

  ```
    sudo systemctl restart php8.2-fpm
  ```

### Configure Nginx
- Open the nginx configuration file:

  ```
    sudo nano /etc/nginx/sites-available/default
  ```

  - Modify the file to look like:

    ```
      server {
          listen 80 default_server;
          listen [::]:80 default_server;
      
          root /var/www/laravel/project-folder/public;
          index index.php index.html index.htm index.nginx-debian.html;
      
          server_name server-ipaddress;
      
          location / {
              try_files $uri $uri/ /index.php?$query_string;
          }
      
          location ~ \.php$ {
              include snippets/fastcgi-php.conf;
              fastcgi_pass unix:/run/php/php8.2-fpm.sock;
          }
      
          location ~ /\.ht {
              deny all;
          }
     }
    ```

    `NOTE:` Replace root /var/www/laravel/`project-folder`/public  as per the name of your project folder name and `server-ipaddress`.

- Confirm the configuration file has no errors:

  ```
    sudo nginx -t
  ```

- Restart nginx:

  ```
    sudo systemctl reload nginx
  ```

### Configure MySql
- Login to the MySql server:

  ```
    mysql -u root -p
  ```

  - Enter the user password when prompted.
 
- Create the project database:

  ```
    CREATE DATABASE project-database;
  ```

- Assign the privileges to the user handling the database:

  ```
    GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';
  ```

- Flush the privileges for the changes to take place:

  ```
    FLUSH PRIVILEGES;
  ```

- Check if the database has been created:

  ```
    SHOW DATABASES;
  ```

### Install Laravel
- Create the directory that will host the Laravel application:

  ```
    sudo mkdir /var/www/laravel && cd /var/www/laravel
  ```

- Clone the project from GitHub:

  ```
    git clone https://github.com/username/project.git
  ```

- Run Composer:

  ```
    composer install --no-dev
  ```

### Permissions
- Change the ownership of the project to the web group:

  ```
    sudo chown -R user:www-data /var/www/laravel/project
  ```

-  Assigning read-write permissions to the storage folder:

   ```
     sudo chmod -R 775 /var/www/laravel/project/storage
   ```

### Configure Laravel
- Change directory to the cloned project folder using `cd /var/www/laravel/project`

- Generate the encryption key for the app:

  ```
    php artisan key:generate
  ```

- Update the credentials in the .env file:

  ```
    sudo nano .env
  ```

  Configurations:

    ```
      APP_ENV=production
      APP_DEBUG=false
      APP_KEY=base64:kGKg6DnMqMGBJrLGDh4Jg+bTIXqcVXZKqJdqueTlkCk=
      APP_URL=http://mydomain.com
      
      DB_HOST=localhost
      DB_DATABASE=yourdatabasename
      DB_USERNAME='root'
      DB_PASSWORD='yourdatabasepassword'
      
      CACHE_DRIVER=file
      SESSION_DRIVER=file
      QUEUE_DRIVER=sync
      
      REDIS_HOST=localhost
      REDIS_PASSWORD=null
      REDIS_PORT=6379
      
      MAIL_DRIVER=smtp
      MAIL_HOST=googlemail.com
      MAIL_PORT=465
      MAIL_USERNAME=XXXXXXXXXXX
      MAIL_PASSWORD=XXXXXXXXXXX
      MAIL_ENCRYPTION=null
    ```

    `NOTE:` Replace `APP_URL` to the registered domain mentioned in the prerequisites, `DB_DATABASE` to the name of the project database, `DB_USERNAME` to the user handling the project database and `DB_PASSWORD` to the mysql user password. Don't change your `APP_KEY` configuration.

- Create a symlink to the public folder to display files:

  ```
    sudo php artisan storage:link
  ```

- Cache necessary configurations to make the app faster:

  ```
    sudo php artisan optimize
  ```

- Migrate the database:

  ```
    sudo php artisan migrate --force
  ```

- Seed the database:

  ```
    sudo php artisan db:seed
  ```

- Acces the Laravel application on your browser using `http://server-ipaddress` or `http://www.project.com`.

### Configure a Secure SSL Certificate
- Install Certbot client:

  ```
    sudo apt-get install certbot python3-certbot-nginx -y
  ```

- Request the SSL certificate for the registered domain:

  ```
    sudo certbot --nginx -d www.project.com
  ```

- Access the app using `https://www.project.com`. 

### Resources
- [How-to-deploy-a-laravel-app-on-lemp-stack](https://www.iankumu.com/blog/how-to-deploy-a-laravel-app-on-lemp-stack/#1-log-in-via-ssh)

- - - - -

## 2. LAMP Stack
  
- - - - -

## 3. Server Hardening

### Debian-based Distros
1. Firewalls

- Makes use of `Uncomplicated Firewall (UFW)` instead of `firewalld`.

```sh
  sudo apt update
  sudo apt install ufw -y # Install UFW
```

```sh
  sudo ufw enable # Enable UFW
  sudo ufw status verbose # Check status of UFW
```

```sh
  sudo ufw allow 443/tcp # Allow tcp rule on UFW
  sudo ufw allow ssh # Allow ssh rule on UFW
  sudo ufw allow http # Allow http rule on UFW
  sudo ufw allow https # Allow https rule on UFW
```

```sh
  sudo ufw delete allow 443/tcp # Delete tcp rule on UFW
  sudo ufw delete allow ssh # Delete ssh rule on UFW
```

```sh
  sudo ufw reload # Reload UFW
```

2. SSL Certificates

- Makes use of `Apache2` instead of `https`.

```sh
  sudo apt install apache2 openssl -y # Install apache2, openssl
  sudo a2enmod ssl # Install mod_ssl
```

```sh
  sudo mkdir -p /etc/ssl/private # Create directory for storing the ssl certs
  sudo chmod 700 /etc/ssl/private # Set the permissions for the directory
```

```sh
  # Generate a self-signed certificate
  sudo openssl req -x509 -nodes -days 365 -newley rsa:2048 \
  -keyout /etc/ssl/private/apache-selfsigned.key \
  -out /etc/ssl/certs/apache-selfsigned.crt
```

```sh
  sudo nano /etc/apache2/sites-available/your_domain_or_ip.conf # Create a new Apache SSL config file

  # In the file:
  # <VirtualHost *:443>
  #  ServerName your_domain_or_ip
  #  DocumentRoot /var/www/html
  #  SSLEngine on
  #  SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
  #  SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
  # </VirtualHost>
```

```sh
  # Enable SSL site and redirect HTTP to HTTPS
  sudo a2ensite your_domain_or_ip.conf
  sudo a2enmod rewrite
  sudo a2enmod ssl
  sudo systemctl reload apache2
```

3. SSH Keys & Configuration

```sh
  ssh-keygen -b 4096 # Generate SSH keys
```

```sh
  scp ~/.ssh/id_rsa.pub username@ipaddress:~/.ssh/authorized_keys # Copy public key to remote server
```

```sh
  # Set correct permissions
  sudo chmod 700 ~/.ssh
  sudo chmod 600 ~/.ssh/authorized_keys
  sudo chown username:username ~/.ssh/authorized_keys
```

```sh
  sudo nano /etc/ssh/sshd_config # Update SSH config

  #  Change:
  #  Port 717
  #  PermitRootLogin no
  #  PasswordAuthentication no
```

```sh
  sudo systemctl restart ssh # Restart SSH service
```

```sh
  ssh username@ipaddress -p 717 # Connect using the new port
```

4. Intrusion Prevention

- Mostly uses `fail2ban` to protect servers from brute-force attacks & other suspicious activities by monitoring log files and implementing automatic IP blocking.

```sh
  sudo apt install fail2ban -y # Install fail2ban
```

```sh
  sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local # Copy the configurarion file
  sudo nano /etc/fail2ban/jail.local # Edit the configuration file
```

```sh
  # Enable and start the fail2ban service
  sudo systemctl enable fail2ban 
  sudo systemctl start fail2ban
  sudo systemctl restart fail2ban 
```

5. AppArmor

- Debian mostly `AppArmor` as the native solution for SELinux, but you can still use SELinux by manually installing it.

```sh
  sudo aa-status # Check AppArmor status
```

```sh
  sudo aa-enforce /etc/apparmor.d/* # Enforce all profiles
```

```sh
  sudo journalctl -xe | grep apparmor # View the logs
```

6. Patch Management

```sh
  sudo apt update # Check for updates
```

```sh
  sudo apt upgrade -y # Upgrade all packages
```

```sh
  sudo apt dist-upgrade -y # Upgrade distribution
```

```sh
  sudo apt autoremove -y # Remove unused packages
  sudo apt clean # Clean the cache
```

7. Log review

```sh
  sudo journalctl # View all logs
```

```sh
  sudo journalctl -f # Follow logs in real time
```

```sh
  sudo journalctl --since "yyyy-mm-dd 00:00:00" --until "yyyy-mm-dd 00:00:00"
```

```sh
  sudo journalctl -p err # Filter logs by priority
```

### RHEL-based Distros
1. Firewalls

- Makes use of `firewalld`.

```sh
  sudo yum install firewalld # Installs the firewall
```

```sh
  sudo systemctl starts firewall # Starts the firewall service
```

```sh
  sudo systemctl enable firewall # Confirms that the firewall service is running
```

```sh
  sudo firewall-cmd --list all
```

```sh
  sudo firewall-cmd --add-port=443/tcp --zone=public --permanent
```

```sh
  sudo firewall-cmd --add-service=ssh --zone=public --permanent
```

```sh
  sudo firewall-cmd --permanent --add-service=http
```

```sh
  sudo firewall-cmd --permanent --add-service=https
```

```sh
  sudo firewall-cmd — get-services
```

```sh
  sudo firewall-cmd --remove-port=443/tcp --zone=public --permanent
```

```sh
  sudo firewall-cmd --remove-service=ssh --zone=public --permanent
```

```sh
  sudo firewall-cmd --reload
```

2. SSL Certificates

```sh
  yum install mod_ssl # Installs the mod_ssl module for Apache
```

```sh
  yum install openssl # For SSl certificate generation and management
```

```sh
  mkdir /etc/ssl/private # Directory for the storing SSL private key
```

```sh
  chmod 700 /etc/ssl/private # Sets the permissions on the private key directory
```

```sh
  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt # Generates a self-signed SSL certificate and private key
```

```sh
  sudo vi etc/httpd/conf.d/(your_domain_or_ip).conf # Specifies the server settings

  # In the file:
  # <VirtualHost *:443>
  #  ServerName your_domain_or_ip
  #  DocumentRoot /var/www/html
  #  SSLEngine on
  #  SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
  #  SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
  # </VirtualHost>

```

```sh
  sudo vi /etc/httpd/conf.d/non-ssl.conf # Specifies the non-SSL settings

  # In the file:
  # <VirtualHost *:80>
  #  ServerName your_domain_or_ip
  #  Redirect "/" "https://your_domain_or_ip/"
  # </VirtualHost>
```

```sh
  sudo apachectl configtest # Checks the Apache configuration files for sysntax errors
```

```sh
  sudo systemctl restart httpd.service # Restarts the Apache service to apply the changes made
```

```sh
  https://your_domain_or_ip # Confirm the status of your website
```

3. SSH Keys & Configuration

```sh
  ssh-keygen -b 4096 # Generate ssh keys
```

```sh
  scp /path/to/public key username@ipaddress:/path/to/store/key # Copy the public key to the remote server
```

```sh
  ssh username@ipaddress # Login using ssh
```

```sh
  sudo chmod 700 .ssh # Set correct permissions
```

```sh
  sudo nano etc/ssh/sshd_config # Update the ssh configuration file

  # In the file:
  # Port - 717
  # PermitRootLogin - no
  # PasswordAuthentication - no
```

```sh
  sudo systemctl restart sshd # Restart sshd to apply the changes made
```

```sh
  ssh username@ipaddress -p 717 # Login using ssh through the port number
```

`N/B:` Incase you get into an error that refuses to log in a user using ssh even after secure copying the public key to the .ssh directory try:

```sh
  sudo chown user:user /home/user/.ssh/authorized_keys
```

```sh
  sudo chmod 600 /home/user/.ssh/authorized_keys
```

4. Intrusion Prevention

- Mostly uses `fail2ban` to protect servers from brute-force attacks & other suspicious activities by monitoring log files and implementing automatic IP blocking.

```sh
  sudo yum install epel-release
  sudo yum install fail2ban
```

```sh
  sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
  sudo nano /etc/fail2ban/jail.local
```

```sh
  sudo systemctl enable fail2ban
```

```sh
  sudo systemctl start fail2ban
  sudo systemctl restart fail2ban
```

5. SELinux (Security Enhanced Linux)

- Security feature integrated into the Linux kernel that provides mandatory access controls for processes and services.

```sh
  sudo sestatus # Check SELinux status
```

```sh
  sudo setenforce 1 # Enable SELinux
  sudo nano /etc/selinux/config
```

```sh
  sudo reboot # Reboot the system
```

```sh
  sudo cat /var/log/audit/audit.log # Review SELinux logs
```

6. Patch Management

```sh
  sudo yum check-update # Check for available updates
```

```sh
  sudo yum update # Update the system
```

```sh
  sudo yum update openssl # Update openssl
```

```sh
  sudo yum autoremove # Find and remove unused packages
```

```sh
  sudo yum clean all # Find and remove temporary files
```

7. Log review

- Makes use of `journalctl` which is a utility enabling users to query and display logs from the systemd journal.

```sh
  sudo journalctl # View all logs
```

```sh
  sudo journalctl -f # Follow real time logs
```

```sh
  sudo journalctl --since "yyyy-mm-dd 00:00:00" --until "yyyy-mm-dd 00:00:00" # Filter logs by time
```

```sh
  sudo journalctl -p err # Filter logs by priority
```

---

## 4. Flask

### Resources
- [How-to-depploy-a-flask-app-using-nginx](https://www.rosehosting.com/blog/how-to-deploy-flask-application-with-nginx-and-gunicorn-on-ubuntu-20-04/)
