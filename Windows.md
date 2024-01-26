# Windows 

# Basic Commands

## Directories
- Create a new directory:
```sh
  mkdir directory_name
```

- Changing directory:
```sh
  cd directory_name
```

- Delete an empty directory:
```sh
  rmdir directory_name
```

- Delete a directory with contents:
```sh
  rmdir /s /q directory_name
```

- List contents in a directory:
```sh
  dir
```

## Files
- Create a new file:
```sh
   echo. > filename.txt
```

```sh
   type nul > filename.txt
```

- Delete a file:
```sh
  del filename.txt
```

## Networking
- View the Wi-Fi network profiles you previously connected to:
```sh
  netsh wlan show profile 
```

- View the password of a Wi-Fi network you previously connected to:
```sh
  netsh wlan show profile wifi_name key=clear
```
