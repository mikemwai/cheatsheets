# Windows 

# Basic Commands

- Checking users in Windows:
```sh
  net user
```

## Files, Folders and Directories Management
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

## Disk Management
- View contents taking up space in your disk:

```sh
  Get-ChildItem -Path $path -Directory |
     ForEach-Object {
         $folder = $_.FullName
         $size = (Get-ChildItem -Path $folder -Recurse -ErrorAction SilentlyContinue |
                  Measure-Object -Property Length -Sum).Sum
         [PSCustomObject]@{
             Folder = $folder
             SizeGB = "{0:N2}" -f ($size / 1GB)
         }
     } |
     Sort-Object SizeGB -Descending |
     Select-Object -First 10 |
     Format-Table -AutoSize
```

## Networking
- Check ip address of the system:

```sh
  ipconfig
```

- Check if specific server is reachable:

```sh
    ping ip_address
```

- Ping with specific number of requests:

```sh
    ping -n no_of_requests ip_address
```

- Continuous ping:

```sh
  ping -t ip_adddress
```

- View the Wi-Fi network profiles you previously connected to:
```sh
  netsh wlan show profile 
```

- View the password of a Wi-Fi network you previously connected to:
```sh
  netsh wlan show profile wifi_name key=clear
```

- Perform a traceroute:

```sh
  tracert ip_address/ hostname
```

## Programs
- Open an app from the command prompt:
```sh
  start appname
```

- List all installed programs:
```sh
  wmic product get name,version,installdate
```

- Upgrade all programs that have updates:
```sh
  winget upgrade --all
```

## VS Code
- Opening a project in a new window:

```sh
  code projectname
```

- Opening a project in a most recently used window:

```sh
  code -r projectname
```

## WSL (Windows Subsystem for Linux)
- Login into your wsl distribution as `root`:
```sh
  wsl -u root
```

- Login into a specific distribution as `root`:
```sh
  wsl -d <DistributionName> -u root
```

- Not sure which distributions you have? Liist them:
```sh
  wsl --list --verbose
```

## Updates
- Check system boot time:
```sh
  systeminfo | find "System Boot Time"
```

- Check updates installed in the system:
```sh
  wmic qfe list
```

```sh
  get-wmiobject -class win32_quickfixengineering
```

- Check descriptions of error codes associated with Windows Updates (Update `0x*******` to the correct error code):
```sh
  certutil -error 0x*******
```

---

- View the PC serial number:
```sh
  wmic bios get serialnumber
```

- View the battery life of your Windows laptop:
```sh
  powercfg /batteryreport
```

- Ever wanted to activate your Windows but don't have an activation code, open powershell and type:
```sh
  irm https://get.activated.win | iex
```
