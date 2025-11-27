# 🐧 Linux Cheatsheet

- `N\B:` When using these commands and you notice they don't run use `sudo` infront of the command.
- This is because:
    - Some distros such as `Ubuntu` have disabled root login.
    - It's simpler and safer for multi-user systems which prevents sharing of the root password.

## 📂 Files, Folders and Directory Management

- File maintenance commands include `cp`, `rm`, `mv`, `mkdir`, `rmdir/ rm -r`, `chgrp`, `chown`.

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
    
    - Deleting directories, sub-directories and its contents:
    
    ```sh
      rm -rf directoryname
    ```

    - Adding content to files:

    ```sh
        echo "content" > filename # Overwrites content in the file with the new content
        echo "content" >> filename # Adds content on the next line
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

- File display commands include `cat`, `more`, `less`, `head`, `tail`.

    - View output of a command one page at a time:
    
    ```sh
        ls -ltr | more # Press space bar to move to the next page & q to exit.
        more filename
        less filename # Press space bar to move to the next page, j to move one line at a time down, k to go back up
    ```
    
    - Display the first 2 lines:
    
    ```sh
        head -2 filename
    ```
    
    - Display the last 10 lines:
    
    ```sh
      tail filename.txt
      tail -1 # Displays the last line
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

    - View contents of a file:
    
    ```sh
      cat file_name
    ```

    - Viewing files and folders in a directory:
    
    ```sh
      ls
    ```
    
    - Viewing files and folders in a directory with detailed information (`l - long format`):
    
    ```sh
      ls -l
      ll
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

    ### Input & Output Redirects

    - Redirects include:
        - `stdin` - (Standard input) Has file descriptor number as 0.
          ```sh
              cat < filename
          ```
          
        - `stdout` - (Standard output) Has file descriptor number as 1.
          ```sh
              ls -l > filename # Overwrites the file output.
              ls -la >> filename # Appends another output to the file.
          ```
          
        - `stderr` - (Standard error) Has file descriptor number as 2.
          ```sh
              telnet hostname 2> errorfile
          ```
    
    - Standard output to a file:
    
        ```sh
            echo "content" | tee filename # Concurrently displays and stores the command's output
            echo "content" | tee -a filename # Concurrently displays, stores and appends the command's output to the file
            ls -l | tee file1 file2 file3 # Store in several files
            tee --help # Display manual for the command
            tee --version # Check the version number for the tee package
        ```

    ### Filters/ Text Processors

    - Commands include `cut`, `awk`, `grep & egrep`, `sort`, `uniq`, `wc`.

        - `cut` allows one to cut parts of lines from specified files & print the result to standard output:
        
        ```sh
            cut --version # Check cut version
            cut -c1 filename # List first letter/ character of the file content in each line
            cut -c1,2,4 filename # Pick & choose letter/ character 1,2, & 4
            cut -c1-5 filename # List range of letters/ characters 1 till 5
            cut -c1-3, 6-8 filename # Lists first 3 characters and the 6th to 8th character
            cut -b1-3 filename # List by byte size (Same as characters)
            cut -d: -f 6 /etc/passwd # List first 6th column right after the :, separated by :
            cut -d, -f 6 filename # List first 6th column right after the ,, separated by ,
            cut -d? -f 6 filename # List first 6th column right after the ?, separated by ?
            cut -d: -f 6-7 /etc/passwd # List first 6th & 7th column right after the :, separated by :
            ls -l | cut -c2-4 # Only print user permissions of files/dir
        ```

        - `awk` allows one to extract fields from a file/ output:
        
        ```sh
            awk --version # Check awk version
            awk '{print $1}' filename # Prints the first column of the file
            awk '{print $2}' filename # Prints the second column of the file
            ls -l | awk '{print $1,$3}' # List 1st & 3rd fields of ls -l output
            ls -l | awk '{print $NF}' # Last field of the output
            awk '/Jerry/ {print}' filename # Search for a specific word (Jerry)
            awk -F: '{print $1}' /etc/passwd # Output only 1st field of /etc/passwd
            echo "Hello Tom" | awk '{$2="Adam"/ print $0}' # Replace field words
            cat file | awk '{$2="Imran"; print $0}' # Replace field words
            awk 'length($0) > 15' file # Get lines that have more than 15 byte size
            ls -l | awk '{if($9 == "seinfeld") print $0;}' # Get the field matching seinfeld in /home/user
            ls -l | awk '{print NF}' # Get the number of fields per line
        ```

        - `grep/egrep (global regular expression print)` processes text line by line & prints any lines which match a specified pattern:

         ```sh
             grep --version # Check grep version
             grep keyword file # Search for a keyword from a file
             grep -c keyword file # Search for a keyword and count
             grep -i keyword file # Search for a keyword ignore case-sensitive
             grep -n keyword file # Display the matched lines and their line numbers
             grep -v keyword file # Display everything but the keyword
             grep keyword file | awk '{print $1}' # Search for a keyword and then only give the 1st field
             ls -l | grep keyword # Search for a keyword & then only give the 1st field
             egrep -i "keyword|keyword2" file # Search for 2 keywords
         ```

            - Complex command chain example:

              ```sh
                  grep vi keyword filename | awk '{print $1}' | cut -c1-3 # Displays everything apart from the keyword ignoring case sensitivty, prints the first column outputs and selects the first 3 characters of the output.
              ```

       - `sort` arranges in alphabetical order.
         
       ```sh
           sort --version or sort --help # Check version/ help
           sort file #  Sorts file in alphabetical order
           sort -r file # Sort in reverse alphabetical order
           sort -k2 file # Sort by field number i.e. 2nd column
       ```
       
       - `uniq` filters out the repeated/ duplicate lines.

       ```sh
           uniq file # Removes duplicates
           sort file | uniq # Sorts first before using uniq
           sort file | uniq -c # Sort first then uniq and list count (Shows how many times the duplicate word appeared)
           sort file | uniq -d # Only show repeated lines
       ```

    ### Compare Files

    - Commands include `diff`, and `cmp`:
 
        - `diff` compares line by line.

        ```sh
            diff file1 file2
            man diff # Displays the different diff options
        ```
        
        - `cmp` compares byte by byte.
     
        ```sh
            cmp file1 file2
            man cmp # Displays the different cmp options 
        ```

- View the current file path:
    
```sh
    pwd # Print Working Directory
```

- Find a file that you forgot where it is:

```sh
    find . -name "filename" # . shows the current directory
    find / -name "filename" # Starts from the root directory hence needs root privilege to run it
    locate filename # Ensure you have installed mlocate package
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

## Help commands

- Show a shorter info version of a command:

```sh
    whatis command
```

- Show a longer info version of a command:

```sh
    command --help
```

- Check for the manual for a command:

```sh
    man command
```

## 🛠️ Common commands

- Check how many characters are in a file:

```sh
    wc -c filename
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

- Command chaining:
    - Executing multiple commands with ;
      ```sh
        ls ; pwd ; whoami
      ```

      - Executing multiple commands with | `(Pipe)`
      ```sh
        ls -ltr | more
      ```

`N/B:` Mostly important for scripting, automation and daily tasks.

- Show the current hostname:

```sh
    hostname
    hostname -f # Display the fully qualified domain name (FQDN)
    hostname -s # Without the domain name
```

- Change the hostname:

```sh
    sudo hostnamectl set-hostname new-hostname
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
