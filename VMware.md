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
