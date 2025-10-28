# 🐧 Linux Cheatsheet

- `N\B:` When using these commands and you notice they don't run use `sudo` infront of the command.
- This is because:
    - Some distros such as `Ubuntu` have disabled root login.
    - It's simpler and safer for multi-user systems which prevents sharing of the root password.

## 📂 Files, Folders and Directory Management

- Creating a new file:

```sh
    touch filename
    touch filename_1 filename_2 filename_3 # Create new files simultaneously
    cp old_filename new_filename
    vi filename # Be careful with this if you do not know how to use the vim editor.
```

- Creating a new directory:

```sh
    mkdir directoryname
    mkdir directoryname_1 directoryname_2 directoryname_3 # Create new directories simultaneously
```

- Create the directory with the full path (just incase one does not exist):

```sh
    mkdir -p directorypath
```

- View the current file path:

```sh
    pwd # Print Working Directory
```

- Viewing files and folders in a directory:

```sh
  ls
```

- Viewing files and folders in a directory with detailed information (`l - long format`):

```sh
  ls -l
```

- Linux Filesystem:
    
```sh
    /boot - Contains the file used by the boot loader (grub.cfg)
    /root - Root user home directory (Different from /)
    /dev - System devices e.g. disk etc
    /etc - Configuration files
    /bin -> /usr/bin - Everyday user commands
    /sbin -> /usr/sbin - System/ filesystem commands
    /opt - Optional add-on apps (Not part of OS apps)
    /proc - Running processes
    /lib -> /usr/lib - C programming librayr files needed by commands and apps
    /tmp - Contains temporary files
    /home - User directory
    /var - System logs
    /run - System daemons that start very early (systemd, udev) storing temporary runtime files like PID files.
    /mnt - Mount external filesystem (e.g. NFS)
    /media - cdrom mounts
```

- List files in long format with human-readable sizes (`h - human readbale sizes`):

```sh
  ls -lh
```

- List files and directories in the current directory in descending form based on modification time (`r - reverse order (starts with older files), t - sort by time`):
  
```sh
  ls -ltr
```

- List files with the permissions and the sizes together with the hour:

```sh
  ls -ltrh
```

- Deleting directories with files in it:

```sh
  rm -rf directoryname
```

- View contents of a file:

```sh
  cat file_name
```

- Display the last 10 lines:

```sh
  tail filename.txt
```

- Display the specific number of lines (i.e. 1000 last lines):

```sh
  tail -n 1000 filename.txt
```

- Display the specific number of bytes (i.e. 500 last bytes):

```sh
  tail -c 500 filename.txt
```

- Handling multiple files:

```sh
  tail filename.txt filename2.log
```

- Display new lines added to the file in real time (i.e. 200 last new lines):

```sh
    tail -200f filename.txt
```

- Copy a file from one location to another:

```sh
    cp source destination
```

- Copy a file to another directory:

```sh
    cp filename directoryname
```

- Copy a directory and its contents:

```sh
    cp -r old_directoryname new_directoryname
```

- Move a file from one location to another:

```sh
    mv source destination
```

- Rename a file:

```sh
    mv old_filename new_filename
```

- Move a directory:

```sh
    mv directoryname new_directory_filepath
```

- Find a file that you forgot where it is:

```sh
    find . -name "filename" # . shows the current directory
    find / -name "filename" # Starts from the root directory hence needs root privilege to run it
    locate filename # Ensure you have installed mlocate package
```

- Changing permissions of a file:

```sh
   # Using letters
   chmod o-w filename # Removes the write permission from others
   chmod g+r filename # Adds the read permission to group
   chmod u+rwx filename # Adds the read, write & execute permissions to user 

   # Levels: 
   # u g o 
   # | | |
   # | | └── Others
   # | └──── Group
   # └────── User

   # Permissions: 
   # r w x 
   # | | |
   # | | └── Execute
   # | └──── Write
   # └────── Read
```

```sh
    # Using numeric mode
    chmod 005 filename # Removes the write permission from others
    chmod 040 filename # Adds the read permission to group
    chmod 700 filename # Adds the read, write & execute permissions to user 

    # Key:
    # 0 - No permission
    # 1 - Execute
    # 2 - Write
    # 3 - Execute + Write
    # 4 - Read
    # 5 - Read + Execute
    # 6 - Read + Write
    # 7 - Read + Write + Execute

    # N\B: Overwites permissions hence good practice to remember the existing permissions of the other levels before changing
```

- Changing ownership of a file:

```sh
    chown owner filename
    chgrp groupname filename

    # Owners:
    # * User * Group

    # Format:
    # owner group
```

## 💽 Disk Usage

- List the disks in the system:

```sh
  lsblk
```

- Check for CPU usage:

```sh
  top
```

- Before using `iostat`, you have to install:

```sh
    sudo apt install sysstat
```

- Check disk I/O capability:

```sh
  iostat
```

- List the files while showing their sizes `du - disk usage`:

```sh
  du -sh *
```

- List the files while showing their sizes `du - disk usage` and if you want those starting with G `grep G`:

```sh
  du -sh * | grep G
```

- Displays disk space usage and availability:

```sh
  df -h
```

- View the disk file system usage in order:

```sh
  df -kh
```

- View the disk file system usage including filesystem type:

```sh
    df -hT
```

- List the disk space:

```sh
    fdisk -l
```

- Partition a disk:

```sh
    fdisk /dev/disk
    n - p - enter- enter - w
    mkfs.xfs /dev/disk
```

- Mount a disk:

```sh
    mkdir new_directory
    fdisk /dev/disk new_directory
```

## 👥 Users Management

- Switch to a specific sudo user `user1`:

```sh
  sudo su - user1
```

- Verify the logged in user:

```sh
  whoami
```

- Switch to the root user (`su - switch user`):
  
```sh
  sudo su -
```

```sh
  sudo -i # Cleaner version of entering to root
```

```sh
  su -
```

- Switch to another non-root user:

```sh
  sudo su - username
```

```sh
  sudo -u username -i # Cleaner version
```

- List the current password aging info for a user account:

```sh
  chage -l username
```

- Manually update password aging settings for a user account:

```sh
  chage username
```

- Add new user

```sh
    sudo useradd -m newusername
```

```sh
    sudo adduser newusername 
```

- Set password for the new user:

```sh
    sudo passwd newusername
```

- Change password:

```sh
    passwd # N/B: It doesn't work if you have forgotten your current password
```

- Change password for another user (Done by `root`):

```sh
    passwd username # N/B: It doesn't work if you have forgotten the user's current password
```

- Modify existing user privileges:

```sh
    sudo usermod -aG groupname username
```

```sh
    sudo visudo # Edit the user privileges
```

- List the sudo users:

```sh
    su - # Then double press the tab key
```

```sh
    cut -d: -f1 /etc/passwd
```

- List the `human` users:

```sh
    ls /home
```

- List the currently logged in users:

```sh
    who
```

### Access Control List (ACL)

- View existing permissions of a file:

```sh
    getfacl filename
```

- Edit the permissions of a file:

```sh
    setfacl -m u:user:rwx /path/to/file # Add permission for user
    setfacl -m g:group:rw /path/to/file # Add permissions for a group

    setfacl -rm "entry" /path/to/dir # Allow all files/ directories to inherit ACL entries from its directory

    setfacl -x u:user /path/to/file # Remove a specific entry for a specific user
    setfacl -b path/to/file # Remove all entries for all users
```

## 🗄️ Database 

### 🦉 Oracle

- Check listener status:

```sh
  lsnrctl status
```

- Check where the cluster services are running from:
  
```sh
  crm status
```

- Switch to oracle:

```sh
  su - oracle
```

- Enter OGG command interpreter:

```sh
  g
```

- Check for OGG lag:

```sh
  info all
```

- View OGG lag info in detail:

```sh
  view report
```

- View more information about a specific process based on the group name:

```sh
  info group_name
```

### 🐬 SQL

- Connect to SQL:

```sh
  sqlplus / as sysdba
```

## 🌐 Networking

- Connect to a server:

```sh
  curl -v telnet://ip_address
```

- Check if specific server is reachable/ Continuous ping:

```sh
    ping ip_address
```

- Ping with specific number of requests:

```sh
    ping -c no_of_requests ip_address
```

- Check ip address for the device:

```sh
    ip addr show
    ip addr
```

```sh
    ifconfig # Works if you have installed net-tools
```

- Perform a traceroute:

```sh
  traceroute ip_address/ hostname
```

## 🔄 Updates Management

- Run updates for a `Debian based distro`:

```sh
  sudo apt update
```

- Download and install updates for each outdated package for a `Debian based distro`:

```sh
  sudo apt upgrade
```

- List the packages that can be upgraded:

```sh
    sudo apt list --upgradable
```

- Install a package for a `Debian based distro`:

```sh
  sudo apt install package
```

- Upgrading the Linux distribution to the next version:

```sh
    sudo apt dist-upgrade -y
```

- Remove packages that are not being used:

```sh
    sudo apt autoremove -y
```

- Delete stored `.deb` cached package files:

```sh
    sudo apt clean
```

## ⚙️ Services Management

- Start a service:

```sh
    sudo systemctl start service
```

- Stop a service

```sh
    sudo systemctl stop service
```

- Enable a service:

```sh
    sudo systemctl enable service
```

- Check status of a service:

```sh
    sudo systemctl status service
```

- Restart a service:

```sh
    sudo systemctl restart service
```

## 🛠️ Common commands

- Check for the manual for a command:

```sh
    man commandname
```

- Ever wondered how to check what Linux distro a server is running? Try running:

```sh
    cat /etc/os-release
```

- Check if keytool is installed:

```sh
  ke
```

- Display list of commands ran in the terminal (audit purposes):

```sh
  history
```

- Check for system uptime:

```sh
    uptime
```

- Restart the system:

```sh
    reboot
```

- Shutting down the system:

```sh
    shutdown
```

- Show the system date and time:

```sh
    date
```

- List current user's cronjobs that are running:

```sh
    crontab -l
```

```sh
    grep CRON /var/log/syslog
```

- Delete all the cron jobs for specific user:

```sh
    crontab -i -r
```

- Open the crontab editor for the current user `Create/ modify scheduled tasks (cronjobs)`:

```sh
    crontab -e
```

```sh
    * * * * * scriptfile/ command-to-run
    | | | | |
    | | | | └── Day of week (0–7) (Sun=0 or 7)
    | | | └──── Month (1–12)
    | | └────── Day of month (1–31)
    | └──────── Hour (0–23)
    └────────── Minute (0–59)
```

- Creating, & executing a script file and adding it as a cron job:

```sh
    nano myscript.sh
```

```sh
    #!/bin/bash
    # This script writes the current date and time into a log file
    
    echo "Script ran at: $(date)" >> /home/yourname/mylog.txt # Edit the command
```

```sh
    chmod +x myscript.sh
```

```sh
    ./myscript.sh
```

```sh
    crontab -e
```

```sh
    * * * * * /home/yourname/myscript.sh
```
