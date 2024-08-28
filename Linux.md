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

- Displays disk space usage and availability:

```sh
  df -h
```

- Lists files and directories in the current directory in an organized manner based on modification time:
  
```sh
  ls -lrt
```

## Common commands

- Run updates for a `Debian based distro`:

```sh
  sudo apt-get update
```

- Switch to a specific sudo user `user1`:

```sh
  sudo su - user1
```

- Verify the logged in user:

```sh
  whoami
```
