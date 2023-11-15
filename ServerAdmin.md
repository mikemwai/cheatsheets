# Server Administration
## Table of Contents

1. [Hosting a Laravel app using the LEMP Stack](#1-lemp-stack)
2. [Hosting a Laravel app using the LAMP Stack](#2-lamp-stack)

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
     
## What is a LEMP Stack?
- It's a combination of four technologies which include Linux, Enginx, MySql and Php. This stack can be used to host web apps built using php frameworks such as Laravel.

## Steps to host the Laravel app
## Login to the Ubuntu Server
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
  
- Logout and login as the new user:
  
  ```
   ssh newuser@ipaddress
  ```
  `NOTE:` `newuser` is the newly created user and `ipaddress` is the server ipaddress. `Avoid running commands using root` for the next steps.
  
## Update the package manager
- Ensure the server is upto date before installing any packages.

   ```
   sudo apt-get update
   ```

## Install Nginx (Enginx)
- Install nginx in your server:
  
  ```
    sudo apt-get install nginx
  ```

- Confirm if it has installed by checking `http://server-ipaddress` on the browser. The nginx welcome page will be displayed.

## Install MySql
- Install the MySql server where the laravel app database will be stored.

  ```
    sudo apt-get install mysql-server
  ```

### Secure MySql
- Secure a database server in production:
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

## Install PHP
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

## Install Composer
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

## Configure PHP
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

## Configure Nginx
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

    `NOTE:` Replace root /var/www/laravel/`project-folder`/public  as per the project folder name and `server-ipaddress`.

- Confirm the configuration file has no errors:

  ```
    sudo nginx -t
  ```

- Restart nginx:

  ```
    sudo systemctl reload nginx
  ```

## Configure MySql
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

## Install Laravel
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

## Permissions
- Change the ownership of the project to the web group:

  ```
    sudo chown -R user:www-data /var/www/laravel/project
  ```

-  Assigning read-write permissions to the storage folder:

   ```
     sudo chmod -R 775 /var/www/laravel/project/storage
   ```

## Configure Laravel
- Change directory to the cloned project folder using `cd /var/www/laravel/project`

- Generate the encryption key for the app:

  ```
    php artisan key:generate
  ```

- Add credentials to the .env file:

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

## Configure a Secure SSL Certificate
- Install Certbot client:

  ```
    sudo apt-get install certbot python3-certbot-nginx -y
  ```

- Request the SSL certificate for the registered domain:

  ```
    sudo certbot --nginx -d www.project.com
  ```

- Access the app using `https://www.project.com`. 

- - - - -

## 2. LAMP Stack
