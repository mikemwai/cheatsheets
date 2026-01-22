# 🐧 Linux Cheatsheet

> [!IMPORTANT]
> - When using these commands and you notice they don't run use `sudo` infront of the command.
> - This is because:
>    - Some distros such as `Ubuntu` have disabled root login.
>    - It's simpler and safer for multi-user systems which prevents sharing of the root password.

## 📂 Files, Folders and Directory Management

### 1) File Maintenance

- The commands include `cp`, `rm`, `mv`, `mkdir`, `rmdir/ rm -r`, `chgrp`, `chown`.

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
    
    ### 2) Access Control List (ACL)
    
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
    
### 3) File Display

- The commands include `cat`, `more`, `less`, `head`, `tail`.

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

    - View newly added content of a file in real time:

    ```sh
        tail -f filename
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
            /run - System daemons that start very early (systemd, udev) storing temporary runtime files like P files.
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

    ### 4) Input & Output Redirects

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

    ### 5) Filters/ Text Processors

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
           sort --version or sort --help # Check version/ help for sort
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

       - `wc` reads standard input/ list of files & generates newline count, word count, and byte count.

       ```sh
            wc --version or wc --help # Check version/help for wc
            man wc # Manual for wc
            wc file # Check file line, word and byte count
            wc -c filename # Check how many characters/ bytes are in a file
            wc -l file # Get the number of lines in a file
            wc -w file # Get the number of words in a file
            ls -l | wc -l # Number of files
            grep keyword | wc -l # Number of keyword lines
       ```

       - Command chain:

       ```sh
           ls -l | grep drw | wc -l
       ```

### 6) Compare Files

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

### 7) File compression and unzipping

- The commands include `tar`, `gzip`, `gunzip`:

  - `tar`
 
  ```sh
      tar cvf zipped_folder_name folder_name # Zip the folder
      tar xvf zipped_folder_name # Unzip the folder
  ```

  - `gzip`

  ```sh
      gzip tar_file # Compresses the tar file
      gzip -d compressed_tar_file or gunzip compressed_tar_file  # Uncompress the compressed tar_file
  ```

### 8) File truncate

- Command used is `truncate` which shrinks/ extends the file size to the specified size.
- `N\B:` Be careful it removes contents in the file i.e. it delete contents.

  ```sh
      truncate -s 10 filename # Reduces the file size to 10 bytes
  ```

### 9) Combining & splitting files

- Command used is `split` which divides a file into multiple files.

```sh
    split -l 2 original_file new_seperated_file # Divides the original file into new files each containing 2 output lines of the original file
```

### 10) Linux File Editor 

- Command used is `vi`:

    ```sh
        vi filename # Create a new file
        man vi # Manual for vim
    ```
    
    - Common keys:
    
        ```sh
            i # Insert
            Esc # Escape out of any mode
            r # Replace
            d # Deletes the entire line
            dd # Deletes and puts the cursor on the next line
            u # Undo an action i.e. deleting
            x # Removes a character
            o # Creates a new line below and automatically puts you into insert mode
            a # Automatically advance to the next space
            :q! # Quit without saving
            :wq! # Quit and save
            Shift + zz # Save a file
            /keyword # Searches for the keyword in the file in vi mode
            :%s/keyword/new_keyword/ # Replace every keyword in a file
        ```
        
- Recover a terminated file from its swap file:

  ```sh
      vi -r filename
  ```

### 11) File Manipulation

- Command used is `sed`:

  ```sh
      sed 's/Keyword/New_keyword/g' filename # Replace a string with a new one on the screen (Doesn't change the actual file)
      sed -i 's/Keyword/New_keyword/g' filename # Insert & Replace a string with a new one in the file (Changes the actual file)
      sed 's/Keyword//g' filename # Remove a string (Doesn't change the actual file)
      sed '/Keyword/d' filename # Delete every line that has the keyword
      sed '/^$/d' filename # Remove empty lines in the file (Doesn't change the actual file)
      sed -i '/^$/d' filename # Remove empty lines in the file (Changes the actual file)
      sed '1d' filename # Remove the first line in a file
      sed '1,2d' filename # Remove the first two lines in a file
      sed 's/\t/ /g' filename # Remove all the tabs with a space in the file
      sed -i 's/\t/ /g' filename # Replaces every tab with a space in the file (Changes the actual file)
      sed -i 's/ /\t/g' filename # Replaces every space in the file with a tab (Changes the actual file)
      sed -n 12,18p filename # Show only lines 12 to 18
      sed 12,18d filename # Show all lines except from line 12 to 18
      sed G filename # Insert an empty line after every line
      sed 's/keyword/new_keyword/' filename # Replaces the keyword with another one for all the lines
      sed '8!s/keyword/new_keyword/g' filename # Replaces the keyword with another one for the rest of the lines except for the keyword in line 8
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

- Find/ Locate the executable file associated with a given command:

```sh
    which command_name
```

### 12) File Permissions & Ownership

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

> - Addtional/ Special Linux permissions include:
>   - `setuid bit` - Tells Linux to run a program with effective user id of the owner instead of the executor i.e. `passwd` command -> `/etc/shadow`.
>   - `setgid bit` - Tells Linux to run a program/ file with executable permissions with effective group id of the owner instead of the executor i.e. `locate` or `wall` command.
>   - `sticky bit` - Bit set on files/ directories allowing only the owner/ root to delete those files.
> - `N\B:` These bits work on c programming executables not on bash shell scripts. Assigned to the last bit of permissions. Helps in preventing users from deleting a directory i.e. `/tmp directory`.

- Find all executables in Linux with setuid and setgid permissions:

```sh
  find / -perm /6000 -type f
```

- Assign special permissions at user level:

```sh
    chmod u+s xyz.sh
```

- Assign special permissions at the group level:

```sh
    chmod g+s xyz.sh
```

- Assign special permissions at the user or group level:

```sh
    chmod u-s xyz.sh
    chmod g-s xyz.sh
```

- Assign a sticky bit permission to a directory:

```sh
    chmod +t /directory
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

- List the files while showing their sizes `du - disk usage`:

```sh
  du -sh *
```

- List the files while showing their sizes `du - disk usage` and if you want those starting with G `grep G`:

```sh
  du -sh * | grep G
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

## 👥 Users' Management

### 1) Users' Creation

- Commands used are `useradd`, `groupadd`, `userdel`, `groupdel`, `usermod`:

  - `useradd`:
    
    ```sh
        sudo useradd -m newusername
        sudo useradd -m newusername
        useradd username # Creates the user using the root user
        useradd -g groupname -s /bin/ash -c "user description" -m -d /home/username username # Includes all the parameters that need to be defined
        man useradd # Manual for useradd
    ```

  `N\B:` After creating the user, set the password for the new user using:

   ```sh
      sudo passwd username
   ```
    
  - `groupadd`:
    
    ```sh
        groupadd groupname # Creates the group using the root user
        man groupadd # Manual for groupadd
    ```

  - `userdel`:

    ```sh
        userdel -r username # Deletes the user using the root user
        man userdel # Manual for userdel
    ```

  - `groupdel`:

    ```sh
        groupdel groupname # Deletes the group using the root user
        man groupdel # Manual for grouodel
    ```

- Files where the user info is stored `/etc/passwd`, `/etc/group`, `/etc/shadow`:

  ```sh
      cat /etc/group # View the created groups
  ```

- Check user details for a certain user:

```sh
     id username
```

- List the `human` users:

```sh
    ls /home
```

### 2) Password Creation

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

### 3) Password Aging

- View the user password paramaters:

    ```sh
        grep username /etc/shadow
    ```
    
    Output:

    ```sh
       username:password:last password change:min days:max days:warn days:inactive days
    ```

- Command used is `chage` and the file is `/etc/login.def`.

    - Combined command if changing for multiple users:

    ```sh
      chage [-m mindays] [-M maxdays] [-d lastday] [-I inactive] [-E expiredate] [-W warndays] user1 user2
      [-m mindays] # Min no. of days required btwn password changes
      [-M maxdays] # Max no. of days password is valid
      [-d lastday] # Last password change
      [-I inactive] # No. of days after password expires that account is disabled
      [-E expiredate] # Absolute date when the login will no longer be used
      [-W warndays] # No. of days before password is to expire that user is warned that their password must be changed
    ```

    - View the contents of the `/etc/login.def`:
 
    ```sh
        more /etc/login.def
    ```

    - Password controls in `/etc/login.def`:

    ```sh
       PASS_MAX_DAYS 999999 # Max no. of days a password may be used
       PASS_MIN_DAYS 0 # Min no. of days allowed btwn password changes
       PASS_MIN_LEN 5 # Min acceptable password length
       PASS_WARN_AGE 7 # No. of days warning given before a password expires
    ```

    - List the current password aging info for a user account:
    
    ```sh
      chage -l username
    ```
    
    - Manually update password aging settings for a user account:
    
    ```sh
      chage username
    ```

### 4) Switching users and sudo access

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

- Switch to a specific sudo user `user1`:

```sh
  sudo su - user1
```

### 5) Monitoring Users

- List the currently logged in users:

```sh
    who
```

```sh
    users
```

- Show all the details of the users since they were logged in:

`1. last`:

```sh
    last | more
    last | awk '{print $1}' | sort | uniq # Lists the all the details of the logged in users, arranged alphabetically while filtering out the repeating values
```

`2. w` - More detailed than `last`:

```sh
    w
```

`3. finger`:

- `N\B:` Finger is not pre-installed hence you need to install it first, then run the command `finger`:

```sh
    yum install finger -y # For CentOS 7
    yum install pinky # For CentOS 8 and above
```

- View the details of a user i.e. group etc:

```sh
    id # For your own logged in user
    id username # For another user
```

### 6) Talking to Users

`1. wall - write all` - Broadcast a message to the terminals of all logged in users:

```sh
    wall
```

 - Then write the message that you want i.e. "Please logoff, This system is coming down for maintenance."
 - Save the message by clicking `Ctrl+d`.

`2. write` - Broadcast a message to the terminal of a specific user:

```sh
    write user_name
```

 - Then write the message that you want i.e. "Please logoff, This system is coming down for maintenance."
 -  To reply to the written message if you're the other user, press `Enter` and run:

```sh
    sudo write user_name
```

 - Save the message by clicking `Ctrl+d`.

### Recover root password
- Restart the computer:

```sh
    reboot
```

- Edit the grub file:
    - For Centos 7, 8:
        - While the boot menu, press `E` on your keyboard on the 1st option which is your actual OS.
        - Scroll down the parameters to the section with `ro`, remove `ro` and write `rw init=/sysroot/bin/sh`. Then do `Ctrl` + `x`. (Starts the computer in `single user mode`)
        - Mounts the system to sys root:
         ```sh
             chroot /sysroot
         ```
         
        - Change the password:
         ```sh
             password root
         ```
    
        - Update selinux info:
         ```sh
             touch /.autorelabel
         ```
    
        - Exit out of the sysroot:
         ```sh
             exit
         ```
    
        - Restart the system:
         ```sh
             reboot
         ```
         
    - For CentOS 9, go to the end of the line and type `rd.break` and do `Ctrl` + `x`.

## Linux Directory Service - Account Authentication

> - Types of Accounts:
>   1) Local Accounts - Stored locally in `/etc/passwd` and `/etc/shadow`.
>   2) Domain/ Directory Accounts - Managed centrally through services like `LDAP - Lightweight Directory Access Protocol`. Makes use of central LDAP directories e.g. Windows = Active Directory, Linux = LDAP.

### Directory Services
| Active Directory | IDM              | WinBIND              | OpenLDAP         | IBM Directory Server |
|------------------|------------------|----------------------|------------------|----------------------|
|Microsoft         |Identity Manager  |Linux/Windows (Samba) |Open source       |IBM                   |

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

## ⚙️ Processes, Signals & Jobs

> - Terms used:
>   1) Application/ Service -  Program running on your computer i.e. NTP, NFS, rsyslog, Apache.
>   2) Script - Instructions written in a file and then packaged in a way that is executable.
>   3) Process - Instance of an application/ service being executed.
>   4) Daemon - Background process running continuously without direct user interaction, waiting for requests/ events like `sshd`, `httpd`.
>   5) Threads - Several smallest sequence of instructions that can be managed indepently by a scheduler.
>   6) Job/ Workorder -  Task that runs a service/ process at a schedule time.
>   7) Signal - Short message sent to a process to interrupt, control or communicate with the process without modifying the code of the program.

### 1) systemctl

- Control system services i.e. start an application/ service:

```sh
    systemctl # RHEL 7 & 8
    service # earlier versions of RHEL
```

- Start a service:

```sh
    sudo systemctl start service
```

- Stop a service

```sh
    sudo systemctl stop service
```

- Check status of a service:

```sh
    sudo systemctl status service
```

- Enable a service:

```sh
    sudo systemctl enable service
```

- Restart a service:

```sh
    sudo systemctl restart service
```

- List all the services/ units available:

    ```sh
        sudo systemctl list-units --all
    ```

    > - The output has the following columns:
    >    - `UNIT` - The service name.
    >    - `LOAD` - Whether the unit's configuration has been parsed by `systemd`. The configuration of loaded units is kept in memory.
    >    - `ACTIVE` - Summary state about whether the unit is active.
    >    - `SUB` - Lower-level state indicating more detailed info about the unit. Often varies by unit type, state, and actual method in which unit runs.
    >    - `DESCRIPTION` - Short textual description of what the unit is/ does.

`N\B:` `units` refer to services in systemctl.

- Add a service under systemctl management (Create a unit file in `/etc/systemd/system/servicename.service`):

```sh
    sudo nano /etc/systemd/system/servicename.service

    # Basic File Structure
    [Unit]
    Description=My Custom Service
    After=network.target
    
    [Service]
    ExecStart=/usr/bin/python3 /path/to/myapp.py
    Restart=always
    User=myuser
    WorkingDirectory=/path/to
    Environment="VAR1=value1" "VAR2=value2"
    
    [Install]
    WantedBy=multi-user.target
```

```sh
    sudo systemctl daemon-reload # Reload systemd to recognize the new unit
```

```sh
    sudo systemctl enable servicename.service # Enable the service
```

```sh
    sudo systemctl start servicename.service # Start the service
    sudo systemctl status servicename # Check the service status
```
 
- Control system with systemctl:

```sh
    systemctl poweroff 
    systemctl halt
    systemctl reboot
```

### 2) ps - process status

- Shows the processes in static view at the time of execution.

- View the currently running processes:

    ```sh
        ps
    ```

    > - The output has the following columns:
    >    - `PID` - The unique process ID.
    >    - `TTY` - Terminal type that the user logged-in to.
    >    - `TIME` - CPU duration that the process has been running.
    >    - `CMD` - Name of the command.

- Show all running processes:

```sh
    ps -e
```

- Show all running processes in full format listing:

```sh
    ps -ef
```

- Show all running processes in BSD format:

```sh
    ps aux
```

- Show all processes by username:

```sh
    ps -u username
```

### 3) kill

- Terminate processes manuallly by sending a signal that terminates a praticular process/ group of processes.

- Terminate a process with default signal:

```sh
    kill pid # pid refers to the process ID.
```

- List all signal names/ numbers:

```sh
    kill -l
```

- Most used signals are:

```sh
    kill -1 pid # Restart a process
    kill -2 pid # Interrupt from the keyboard just like Ctrl C
    kill -9 pid # Forecefully kill the process
    kill -15 pid # Kill a process gracefully
```

- Other similar kill commands:

```sh
    killall # Terminate all parents processes with their children processes
    pkill process_name# Terminate by the process name
```

### 4) crontab

- Schedule processes/services:

```sh
    crontab
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

- Manage the crond service `crond - daemon/ service managing scheduling`:

```sh
    systemctl status crond
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
    vi myscript.sh
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

> - Additional cron jobs:
>   - Hourly
>   - Daily
>   - Weekly
>   - Monthly
> - All the above crons are setup in: `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weekly`, & `/etc/cron.monthly`.
> - The timing for each are set in `/etc/anacrontab` except hourly.
> - For hourly `/etc/cron.d/0hourly`.

### 5) at

- Schedule processes/services on a one time basis (only once):

```sh
    at hh:mm pm/ am 
```

`N\B:` To get out of the interactive mode, you press `Ctrl` + `d`.

- List all the entries in the at table:

```sh
    atq
```

- Remove an entry in the at table:

```sh
    atrm entry_no
```

- Manage the atd service `atd - daemon/ service managing scheduling`:

```sh
    systemctl status atd
```

- Other scheduling formats:

```sh
    at 2:45 AM 101621 # Schedule a job to run on Oct 16th, 2021 at 2:45am
    at 4PM + 4 days # Schedule a job at 4pm four days from now
    at now +5 hours # Schedule a job to run five hours from now
    at 8:00 AM Sun # Schedule a job to 8am on coming Sunday
    at 10:00 AM next month # Schedule a job to 10am next month
```

### 6) top 

- Shows the running processes in real-time view.

- Check for CPU usage:

```sh
  top
```

> - The output has the following columns:
>    - `PID` - Shows task's unique process id.
>    - `USER` - Username of task owner.
>    - `PR` - Shows the scheduling priority of the process from the kernel perspective.
>    - `NI` - Represents a Nice Value of a task. A Negative nice value implies higher priority & positive means lower priority.
>    - `VIRT` - Total virtual memory used by the task.
>    - `RES` - Memory consumed by the process in RAM.
>    - `SHR` - Represents the shared memory amount used by a task.
>    - `S` - Shows the process state in the single-letter form.
>    - `%CPU` - Represents the CPU usage.
>    - `%MEM` - Shows the memory usage of task.
>    - `TIME+` - CPU Time, but reflects more granulariy through hundredths of a second.
>    - `COMMAND` - Actual process.

`N\B:` To exit out of the interactive mode, click `q`. Moreover, the info displayed is refreshed every 3 secs.

- Show tasks/ processes owned by a user:

```sh
    top -u username
```

- Show commands absolute path:

    ```sh
        top 
    ```

    - Then press `c`.
 
- Kill a process by PID within top session:

    ```sh
        top 
    ```

    - Then press `k`.
 
- Sort all running processes by memory usage:

    ```sh
        top 
    ```

    - Then press `Shift` + `M` and `Shift` + `P`.

### Process Signals

> - Used to send signals to running processes to control their behaviour.
> - Purpose:
>    - Enable communication between the user and the operating system allowing them to manage processes in real time.
> - `Types of process signals:`
>    1) `Standard Signals`
>       - Predefined signals that the OS send to processes to control their behaviour.
>       - These signals have meaning: stop, pause, or terminate.
>       - `Types of standard signals`: i) `SIGINT (Signal Interrupt)`
>                                      ii) `SIGTERM (Signal Terminate)`
>                                      iii) `SIGKILL (Signal Kill)`
>                                      iv) `SIGSTOP (Signal Stop)`
>                                      v) `SIGCONT (Signal Continue)`
>                                      vi) `SIGSEGV (Signal Segmentation Fault)`
>       `N\B:` They are sent one by one without any order.
>
>    2) `Real time Signals`
>       - Allows processes to send extra information along with a signal such as numbers or messages making them useful for advacned tasks.
>       - They are queued i.e. sent one by one in an order.
>       - `Types of real time signals`: i) `SIGRTMIN (Signal Real Time Minimum)`
>                                      ii) `SIGRTMAX (Signal Real Time Maximum)`
>                                      iii) `Intermediate Signals`

### A) Standard Signals
`1) SIGINT (Signal Interrupt)`

- Used to interrupt a process when the user wants to stop it gracefully (Like using Ctrl + C to close a program i.e. word):

```sh
  kill -sigint pid
  Ctrl + C
```

`2) SIGTERM (Signal Terminate)`

- Process to terminate gracefully which saves the data, and cleans up before closing (Like using the menu to close a program i.e. word):

```sh
  kill -sigterm pid
```

`N\B:` This creates a swap file for the terminated file.

`3) SIGKILL (Signal Kill)`

- Forecfully kills a process immediately (Like using the task manager to close a program i.e. word):

```sh
  kill -sigkill pid
```

`4) SIGSTOP (Signal Stop)`

- Pause running process (Like using a pause editor to stop a program i.e. word):

```sh
  kill -sigstop pid
```

`5) SIGCONT (Signal Continue)`

- Resume stopped process (Unpause the editor), run the command on the terminal session where the stopped process was running:

```sh
  kill -sigcont pid
```

- Bring a background process to the foreground of the current terminal session:

```sh
    fg
```

`6) SIGSEGV (Signal Segmentation Fault)`

- Sent to a process when it tries to access an invalid memory location which causes a crash `Segmentation Fault` (Like opening a file that doesn't exist in memory):

```sh
  kill -sigsegv pid
```

### B) Real Time Signals
`1) SIGRTMIN (Signal Real Time Minimum)`

- Marks the beginning of the real time signal range i.e. any real time signal starts from this point (When a system starts it sends this signal to turn it on):

```sh
  kill -sigint pid
  Ctrl + C
```

`N\B:` Signal 0 is commonly used to check if a process is alive without interrupting it.

`2) SIGRTMAX (Signal Real Time Maximum)`

- Marks the end of the real time signal range i.e. highest number in this range (When a system shuts down it sends this signal to turn it off):

```sh
  kill -sigint pid
  Ctrl + C
```

`3) Intermediate Signal`

- Used for custom processes i.e. allow running programs to send specific messages/ data to each other (In btwn the signals they are used for specific tasks e.g. one can be used to turn on the lights, adjust the A/C, or tell the CCTV to start recording):

```sh
  kill -sigint pid
  Ctrl + C
```

### Process Management

- Put a process in the background:

```sh
    `Ctrl` + `z`
    jobs # View the stopped processes
    bg
```

- Bring a process in the foreground:

```sh
    fg
```

- `N/B:` Click `Ctrl` + `C` to bring back your prompt, however the background process will be killed. 

- Run process even after exiting the terminal:

    ```sh
        nohup process & # A message is displayed on the prompt by the `nohup` command.
        nohup process > /dev/null 2>&1 & # Doesn't display the nohup message.
    ```

    - Example:

        ```sh
            nohup sleep 75 & 
        ```

- `N/B:` A `nohup.out` file is generated and it contains all the recorded info. 
  
- Prioritise a process:

```sh
    nice -n 5 process_name
```

- `N/B:` The niceness scale goes from `-20` to `19`. The lower the number, more priority that task gets.

## System Monitoring

`1) df` - Disk space usage
- Shows the disk partition information:

```sh
    df
    df -h # Human readable format
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
  
`2) dmesg`
- Shows the output of system related warnings, error messages or failures:

```sh
    dmesg
```

`3) netstat/ ip & ss`
- Print network connections, routing tables etc:

```sh
    netstat
    ip & ss # In-depth investigation
```

`4) free` - Memory statistics
- Show the physical memory and your swap (Virtual memory):

```sh
    free
```

`5) cat /proc/cpuinfo`
- View the details of your CPU:

```sh
    cat /proc/cpuinfo
```

- `N/B:` `proc` directory keeps all the system information.

`6) cat /proc/meminfo`
- View the details of your memory:

```sh
    cat /proc/meminfo
```

`7) top` - CPU & memory usage

`8) iostat` - CPU & I/O performance
- View the input/ output statistics/ capability:

```sh
  iostat
```

- Before using `iostat`, you have to install:

```sh
    sudo apt install sysstat # Debian based
    sudo zypper install sysstat # Open Suse based
    sudo yum/dnf install sysstat # RPM based 
```

- View the I/O statistics after every certain time duration i.e. 1 second:

```sh
    iostat time_duration # Example: iostat 1
```

- `N/B:` Enter `Ctrl` + `C` to quit and return back to your prommpt.

`9) ip route`
- View the routing information i.e. gateway information, source ip address assigned to your interface:

```sh
    ip route
    ip route | column -t # Human readable format
```

`10) ss`
- View detailed information of the routing information showing active connections of the system, including their state, local & peer addresses, & associated processes:

```sh
    ss | more
```

## Log Monitoring

> - Way of keeping record of the system activities.
> - Log directory is `/var/log`.
> - Examples of logs: `boot`, `chronyd`, `cron`, `maillog`, `secure`, `messages`, `httpd`.

`boot`
- View the boot log file (Records booting activity):

```sh
    more boot.log
```

`cron`
- View the cron log file:

```sh
    more cron.log
```

`secure`
- View the secure log file (Records loggging in and out activities):

```sh
    more secure.log
    tail -f secure.log # View newly added logs in real time
```

`messages`
- View the messages log file (Records system issues):

```sh
    more messages.log
    grep -i error messages # Ignore case senstivity and find lines containing `error`.
```

## System Maintenance
- Commands that are used to bring the system to shutdown, reboot, single mode etc include `shutdown`, `init`, `reboot`, `halt`.

`1) shutdown`
- Power off the system gracefully:
```sh
  shutdown
    shutdown -h now # Power off immediately
```

`2) init`
```sh
  init no
```

- `N/B:` `init` run levels are from 0-6, with `0` for shutdown, `6` for reboot, and `3` for multi user mode.

`3) reboot`
- Restart the system:
```sh
  reboot
```

`4) halt` - Leaves the system powered on
- Shutdowns the system right away without waiting for running processes to stop:
```sh
  halt
```

## System Hostname
- View the hostname of your system:

```sh
    hostname
    hostname -f # Display the fully qualified domain name (FQDN)
    hostname -s # Without the domain name
```

- Change the hostname:

```sh
    sudo hostnamectl set-hostname new-hostname
```

- Reboot the system for the changes to be applied:

```sh
    reboot
```

- `N/B:` The file used to save the new hostname is `/etc/hostname` or `/etc/sysconfig/network`.

## System Information
- Show the operating system name being used:

```sh
    uname
    uname -a # Gives more details i.e. OS, hostname, kernel version, build date
```

- Read information about a computer's hardware:

```sh
    dmidecode
```

- View the system architecture:

```sh
    arch
```

## 🌐 Networking
### 1) Miscellaneous
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

### 2) Hostname/ IP lookup 
- Makes use of `nslookup` and `dig`:
  
    - `nslookup`:
      
    ```sh
        nslookup www.google.com # Update this part
    ```

    - `dig`:
  
    ```sh
        dig www.google.com # This one gives a more detailed output
    ```

### 3) Network Time Protocol (NTP) 
- For time synchronization, runs on `port no. 123`:

    ```sh
        - rpm -qa | grep ntp # Check if ntp has been installed
        - yum install ntp # Install the ntp package if not installed
        - vi /etc/ntp.conf # Modify the configuration file
    
        - In the file, edit the server to 8.8.8.8 (Google's DNS server for the correct time):
            # Please consider joining the pool....
            server 8.8.8.8
        
            Save the file...esc...:wq!
    
        - systemctl start ntpd # Starts the service to save the changes done
        - systemctl status ntpd # Confirm that the service is running
          ps -ef | grep ntp # Check if the service is running
    ```
    
    `N\B`:
    ```sh
        systemctl enable ntpd # If not started, enable it to start at boot
        systemctl stop ntpd # Stops the ntp service
    ```
    
    - In the ntpq interactive command line:
    ```sh
        ntpq # Takes you to the interactive ntp command line
        peers # Shows you the servers you are connected to
        quit # Exit
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

## SOS Report
- It collects and packages diagnostic and support data:

```sh
    sosreport
```

- View the version:

```sh
    sos-version
```

- `N/B:` The files are saved by default under `/var/tmp/`.

## Terminal Control Keys
- Erase everything typed on the command line:

```sh
    Ctrl + u
```

- Stop/ kill a command:

```sh
    Ctrl + c
```

- Suspend a command (Puts process in the background):

```sh
    Ctrl + z
```

- Exit from an interactive program (signals end of data):

```sh
    Ctrl + d
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

## 🛠️ System Utility/ Terminal Commands

- Show the time/ date:

```sh
    date
```

- Show how long the system has been up, no. of logged in users, and average CPU load:

```sh
    uptime
```

- Show the calendar:

```sh
    cal
    cal month_no year # Display the calendar for a specific month for a specific year
    cal year # Display the calendar for a specific year
```

- `bc - binary calculator` - Perform mathematical calculations:
  
```sh
    bc
```

- Clear your screen:

```sh
    clear
```

- Get out of the shell, terminal, user session:

```sh
    exit
```

- Store terminal activities in a log file that is named by the user:

```sh
    script
```

- `N/B:` Default filename if not named is `typescript`.

## GNU Screen
- Used for managing multiple terminal sessions in a single window.
- Verify if screen has been installed:

```sh
    rpm -qa | grep screen
```

- Install the screen package:

```sh
    sudo dnf install epel-release
    sudo dnf install screen
```

- Use screen:

```sh
    screen
```

- Split the terminal screen vertically:

```sh
    `Alt` + `A`
    `Shift` + `|`
```

- Split the terminal screen horizontally:

```sh
    `Alt` + `A`
    `Shift` + `S`
```

- Switch between the different screens:

```sh
    `Alt` + `A`
    `Tab`
```

- Activate an inactive screen:

```sh
    `Alt` + `A`
    `C`
```

- View previously detached screen sessions to reconnect to:

```sh
    screen -r
    screen -r pid
```

## Terminal Multiplex 
- Screen utility allowing one to run multiple terminal sessions in a single window (For CentOS and above).
- Verify if screen has been installed:

```sh
    rpm -qa | grep tmux
```

- Install tmux package:

```sh
    sudo dnf install tmux -y # Installs tmux and answers y to the prompts
```

- Use tmux:

```sh
    tmux
```

- `N/B:` `[0]` shows the current session number, `0:bash` shows window 0, and `*` shows the active window which are displayed at the bottom of the screen.

- Create a vertical split in tmux:

```sh
    `Ctrl` + `b`
    `Shift` + `%`
```

- Create a horizontal split in tmux:

```sh
    `Ctrl` + `b`
    `Shift` + `"`
```

- Switch between the screens:

```sh
    `Ctrl` + `b`
    `Arrow keys` # Choose the correct key based on the direction of your screen
```

- Quit tmux without closing it:

```sh
    `Ctrl` + `b`
    `d`
```

- Check all active tmux sessions:

```sh
    tmux ls
```

- Return back to tmux:

```sh
    tmux a
```

- Create a tmux session with a custom name:

```sh
    tmux new -s custom_name
```

- Create multiple tmux terminals in the same session without splitting the screen:

```sh
    `Ctrl` + `b`
    `c`
```

- Switch between multiple tmux terminals within the same session:

```sh
    `Ctrl` + `b`
    `n`
```

- Permanently close a window in tmux:

```sh
    `Ctrl` + `d`
```

- Return to a certain tmux session:

```sh
    tmux attach-session -t session_name
```

- Rename a current window in tmux:

```sh
    `Ctrl` + `b`
    `,`
    new_name
```

- Terminate a tmux session:

```sh
    tmux kill-session -t session_name
```

## Environment Variables
- Set of defined rules and values to build an environment.
- View all environment variables:

```sh
    printenv or env
```

- View one environment variable:

```sh
    echo $SHELL # Update $SHELL to the variable you want
```

- Set the environment variables:

```sh
    export TEST=1
    echo $TEST
```

- Set environment variable permanently:

```sh
    vi .bashrc
    TEST=`123`
    export TEST
```

- `N/B:` Log out of the session and log in again for the change to be applied.

- Set global environment variable permanently:

```sh
    vi /etc/profile or /etc/bashrc
    Test=`123`
    export TEST
```

## Shell Scripting
- `Kernel` - Interface between hardware and software, forwards commands from the shell to the hardware.
- `Shell` - Interface between users and kernel.
- `Shell script` - Executable file containing multiple shell commands that are executed sequentially.
- Find the shell:

```sh
    echo $0
```

- List all available shells:

```sh
    cat /etc/shells
    cat /etc/passwd # Shows the type of shell assigned to a user
```

> #### Types of Linux Shells
> - Examples: `Gnome`, `KDE`, `sh`, `bash`, `csh and tcsh`, `ksh`
>
> #### Contents of shell script
>   1) `Shell` (#!/bin/bash) - The 1st line of a shell script file.
>   2) `Comments` (# comments) - Description of the script.
>   3) `Commands` (echo, cp, grep etc.)
>   4) `Statements` (if, while, for etc.)
>
> - `N/B:`- A shell script should have executable permissions e.g. -rwx, r-x.
>         - A shell script has to be called from the absolute path.

### Basic Scripts
`1) Output to screen using echo`
- Create the directory `myscripts`:

```sh
    mkdir myscripts
    cd myscripts
```

- Create the shell script:

```sh
    vi output-screen
```

```sh
    #!/bin/bash

    echo Hello World
```

- Change the file permissions to allow it to be executable:

```sh
    chmod a+x output-screen
    ls -ltr # Confirm the file is now executable
```

- Run the shell script:

```sh
    ./output-screen
```

`2) Defining small tasks`
```sh
    #!/bin/bash
    # Defines small tasks
    whoami
    echo
    pwd
    echo # Creates a blank line
    hostname
    echo # Creates a blank line
    ls -ltr
    echo # Creates a blank line
```

`3) Defining variables`
```sh
    #!/bin/bash
    # Defines variables

    a=welcome
    b=to
    c='Linux course' # Single quote since there is a space in between

    echo "Hi $a"
    echo "$b my"
    echo "$c"
```

`3) Input/Output`
```sh
    #!/bin/bash
    # Input/Output

    echo Hello, my name is Linux
    echo
    echo What is your name?
    read name
    echo
    echo Hello $name
    echo
```

```sh
    #!/bin/bash
    # Input/Output

    a=`hostname` # The ticks (``) allow you to run an actual Linux command

    echo Hello, my name is $a
    echo
    echo What is your name?
    read b
    echo
    echo Hello $b
    echo
```

## 🛠️ Common commands

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

- Ever found yourself in a situation where your keyboard inputs a different key as shown?, try these:

```sh
    sudo localectl status # Check the current layout especially X11 Layout
    sudo localectl set-keymap layout # Switch to the correct layout of your keyboard i.e. us, gb etc
    sudo localectl set-x11-keymap layout # Switch to the correct layout of your keyboard i.e. us, gb etc
    reboot # For the changes to be saved
    sudo localectl status # Verify the changes have happened
```

- Pause the execution of a script/ process for a specified time period:

```sh
    sleep no_of_seconds
```
