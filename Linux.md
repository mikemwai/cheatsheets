# Linux Cheatsheet

- `N\B:` When using these commands and you notice they don't run use `sudo` infront of the command.
- This is because:
    - Some distros such as `Ubuntu` have disabled root login.
    - It's simpler and safer for multi-user systems which prevents sharing of the root password.

## Files, Folders and Directory Management

- Creating a new file:

```sh
  touch filename
```

- Creating a new directory:

```sh
  mkdir directoryname
```

- View the current file path:

```sh
    pwd // Print Working Directory
```

- Viewing files and folders in a directory:

```sh
  ls
```

- Viewing files and folders in a directory with detailed information (`l - long format`):

```sh
  ls -l
```

- List files in long format with human-readable sizes (`h - human readbale sizes`):

```sh
  ls -lh
```

- List files and directories in the current directory in an organized manner based on modification time (`r - reverse order (starts with older files), t - sort by time`):
  
```sh
  ls -lrt
```

- List files with the permissions and the sizes together with the hour:

```sh
  ls -ltrh
```

- Deleting directories with files in it:

```sh
  rm -rf directoryname
```

- List the files while showing their sizes `du - disk usage`:

```sh
  du -sh *
```

- List the files while showing their sizes `du - disk usage` and if you want those starting with G `grep G`:

```sh
  du -sh * | grep G
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

- Display the specific number of bytes:

```sh
  tail -c 500 filename.txt
```

- Handling multiple files:

```sh
  tail filename.txt filename2.log
```

## Disk Usage

- List the disks in the system:

```sh
  lsblk
```

- Check for CPU usage:

```sh
  top
```

- Check disk I/O capability:

```sh
  iostat
```

- Displays disk space usage and availability:

```sh
  df -h
```

- View the disk usage in order:

```sh
  df -kh
```

## Users Management

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
  sudo -i // Cleaner version of entering to root
```

```sh
  su -
```

- Switch to another non-root user:

```sh
  sudo su - username
```

```sh
  sudo -u username -i // Cleaner version
```

- List the current password aging info for a user account:

```sh
  chage -l username
```

- Manually update password aging settings for a user account:

```sh
  chage username
```

## Database 

### Oracle

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

### SQL

- Connect to SQL:

```sh
  sqlplus / as sysdba
```

## Networking

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

## Common commands

- Run updates for a `Debian based distro`:

```sh
  sudo apt update
```

- Download and install updates for each outdated package for a `Debian based distro`:

```sh
  sudo apt upgrade
```

- Install a package for a `Debian based distro`:

```sh
  sudo apt install package
```

- Check if keytool is installed:

```sh
  ke
```

- Display list of commands ran in the terminal (audit purposes):

```sh
  history
```
