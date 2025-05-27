# Linux Cheatsheet

## Files and Folders

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

- Viewing files and folders in a directory with detailed information:

```sh
  ls -l
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

- List the disks in the system:

```sh
  lsblk
```

- Lists files and directories in the current directory in an organized manner based on modification time:
  
```sh
  ls -lrt
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

---

```sh
  ls -lh
```

```sh
  rm -rf
```
