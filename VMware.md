# VMware Cheatsheet
## A) vCenter Console
### Services
- Check if services are running:

```sh
  service-control --status
```

- Stop the running services:

```sh
  service-control --stop --all # Stops all the running services
  service-control --stop service_name # Starts a specific service name
```

- Start the running services:

```sh
  service-control --start --all # Starts all the running services
  service-control --start service_name # Starts a specific service name
```

- List all the services:

```sh
  service-control --list-services 
```

- Ever encountered this error:

```sh
  Service vcha startup type is not automatic. Skip
```

Solution:

```sh
  shell
  cd /usr/lib/vmware-vmon
  ./vmon-cli -s vcha
  ./vmon-cli -S AUTOMATIC -U vcha
  ./vmon-cli -s vcha
  exit
  service-control --start vmware-vcha
```

---

### Other Commands
- Enter into bash:

```sh
  shell
```

## B) ESXi Console
### Other Commands
- Check the ip address:

```sh
  esxcli network ip interface ipv4 get
```

## C) VM Player
- Ever found yourself in situation where you can't run another VM inside your ESXi VM? try running this in powershell as administrator:

  ```sh
    bcdedit /set hypervisorlaunchtype off # Disables Hyper-V completely
  ```

  - Turn off Core isolation/ Memory Integrity in Windows Security
