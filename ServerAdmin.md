# Table of Contents

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
- It's a combination of 4 technologies which include Linux, Enginx, MySql and Php. This stack can be used to host web apps built using php frameworks such as Laravel.

## Steps to host the Laravel app
## a) Login to the Ubuntu Server
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
  
## b) Update the package manager
- Ensure the server is upto date before installing any packages.

   ```
   sudo apt-get update
   ```

## c) Install Nginx (Enginx)
- Install nginx in your server:
  
  ```
    sudo apt-get install nginx
  ```

- Confirm if it has installed by checking `http://server-ipaddress` on the browser. The nginx welcome page will be displayed.

## d) Install MySql
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

## e) Install PHP
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

## f) Install Composer
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

## g) Configure PHP
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

## h) Configure Nginx

- - - - -

## 2. LAMP Stack
