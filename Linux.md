# Linux Cheatsheet

## Files, Folders and Disk Usage

- Creating a new file:

```sh
  touch filename
```

- Creating a new directory:

```sh
  mkdir directoryname
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

- List files and directories in the current directory in an organized manner based on modification time (`r - reverse order (starts with older files, t - sort by time)`):
  
```sh
  ls -lrt
```

- List files with the permissions and the sizes together with the hour:

```sh
  ls -ltrh
```

- List the disks in the system:

```sh
  lsblk
```

- Deleting directories with files in it:

```sh
  rm -rf directoryname
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

- Check for CPU usage:

```sh
  top
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

## Common commands

- Run updates for a `Debian based distro`:

```sh
  sudo apt update
```

- Download and install updates for each outdated package for a `Debian based distro`:

```sh
  sudo apt upgrade
```

- Switch to a specific sudo user `user1`:

```sh
  sudo su - user1
```

- Verify the logged in user:

```sh
  whoami
```

- Install a package for a `Debian based distro`:

```sh
  sudo apt install package
```

- Switch to the root user:
  
```sh
  sudo su -
```

```sh
  sudo -i
```

- Check if keytool is installed:

```sh
  ke
```

- Display list of commands ran in the terminal (audit purposes):

```sh
  history
```

---

```sh
  rm -rf
```

```sh
  su -
```

```sh
  chage
```

```sh
  chage -l
```
