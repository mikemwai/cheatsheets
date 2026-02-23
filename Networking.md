# 🌐 Networking

> - Network Components:
>   - `IP`
>   - `Subnet mask` - A 32-bit number masking an IP address, & divides it into network address and host address. Done by setting network bits to all "1"s & setting host bits to all "0"s e.g. `255 = 11111111`, `0 = 0`.
>   - `Gateway` - Tells the computer which route to pick when sending traffic in and out i.e. switch.
>   - `Static vs DHCP`
>   - `Interface`
>
> - Address Classes:
>   - `Class A` - Subnet Mask is `255.0.0.0`.
>   - `Class B` - Subnet Mask is `255.255.0.0`.
>   - `Class C` - Subnet Mask is `255.255.255.0`.

## Network Protocols and their port numbers

|Network Protocol  | Port Number      |
|------------------|------------------|
|DNS (TCP/UDP)     |53                |
|DHCP              |67/68             |
|FTP/SCP/rsync     |21                |
|HTTP              |80                |
|HTTPS             |443               |
|IMAP              |143/993           |
|NTP               |123               |
|RDP               |3389              |
|SMTP              |25                |
|SSH               |22                |
|Telnet            |23                |

## 🐧Linux
> - Interface configuration files include:
>    - `/etc/nsswitch.conf` - Tells the system where it should resolve it's host name to IP address.
>    - `/etc/hosts` - Where you define your IP address and system host name. Acts like a DNS where you can give a name to a certain IP address and you can ping using that name.
>    - `/etc/sysconfig/network` - Where you specify your host name.
>    - `/etc/sysconfig/network-scripts` - Where you specify your IP address on all the networks.
>    - `/etc/sysconfig/network-scripts-ifcfg-interface` - Where you specify the interface details.
>    - `/etc/resolv.conf` - Specifies your DNS server i.e. resolves host name to IP address and vice versa. (Default port number for DNS is `53`)

### 1) Miscellaneous
- Connect to a server using a url:

```sh
  curl -v telnet://ip_address # Connects to a server via the Telnet protocol.
  curl http://website.com/filename # Downloads a file from a web server using the HTTP protocol.
  curl -O http://website.com/filename # Downloads the file.
```

> - The default port number for Telnet is `23`.

- Check if specific server is reachable/ Continuous ping:

```sh
    ping ip_address
```

- Ping with specific number of requests:

```sh
    ping -c no_of_requests ip_address
```

- Perform a traceroute:

```sh
  traceroute ip_address/ hostname
```

- Bring up a network interface

```sh
    if up
```

- Bring down a network interface:
  
```sh
    if up
```

- Show your gateway, how traffic is flowing & which interfaces it's flowing from:

```sh
    netstat -rnv
```

- Trace/ Listen every single transaction leaving & entering your machine (Acts as a sniffing tool):

```sh
    tcpdump -i interface_name # Check under the Iface column
```

### 2) Hostname/ IP lookup 
- Makes use of `nslookup` and `dig`:
  
    - `nslookup` used to find out what website is using the ip address:
      
    ```sh
        nslookup www.google.com # Update this part
    ```

    - `dig`:
  
    ```sh
        dig www.google.com # This one gives a more detailed output
    ```

### 3) Network Time Protocol (NTP) 
> - Used for time synchronization, and NTP runs on `port no. 123`.
> - The configuration file is `/etc/ntp.conf`.

- Configuring NTP:
  
    ```sh
        rpm -qa | grep ntp # Check if ntp has been installed
        yum install ntp # Install the ntp package if not installed
        vi /etc/ntp.conf # Modify the configuration file
    ```
    
    - In the file, edit the server to 8.8.8.8 (Google's DNS server for the correct time):
      
    ```sh
      # Please consider joining the pool....
      server 8.8.8.8
  
      # Save the file...esc...:wq!
    ```

    ```sh
       systemctl start ntpd # Starts the service to save the changes done
       systemctl status ntpd # Confirm that the service is running
       ps -ef | grep ntp # Check if the service is running
    ```
    
  - `N\B`:
    
  ```sh
      systemctl enable ntpd # If not started, enable it to start at boot
      systemctl stop ntpd # Stops the ntp service
      systemctl restart ntpd # Restarts the ntp service
  ```
    
  - In the ntpq interactive command line:
    
  ```sh
      ntpq # Takes you to the interactive ntp command line
      peers # Shows you the servers you are connected to
      quit # Exit
  ```

#### Chronyd (New Version of NTP)
> - Package name is `chrony`.
> - The configuration file is `/etc/chrony.conf`.
> - The log file is `/var/log/chrony`.
> - The service is `systemctl start/restart chronyd`.
> - The program command is `chronyc`.
> - `N/B:` Chronyd and NTP cannot run concurrently, choose only one service to run. Make sure you have stopped and disabled the other service.

- Confirm if the `chrony` package has been installed:

  ```sh
    rpm -qa | grep chrony
    yum install chrony # If chrony package has not been installed.
  ```

- Configuring chronyd:

  - Edit the configuration file:

  ```sh
    vi /etc/chrony.conf

    # Scroll to the section beginning with and add the line:
    # Please consider joining the pool...
    server 8.8.8.8 # Google's DNS server

    # Save the file: wq!
  ```

  - Start the service:

  ```sh
    systemctl status chronyd # Confirm if the service is running
    systemctl start chronyd # Start the chronyd service
    systemctl enable chronyd # Enable the chronyd service to start at boot time
    systemctl restart chronyd # Use the restart command anytime you modify the configuration file...this is after you've started and enabled it
  ```

- Enter the interactive mode for chrony:

```sh
  chronyc
  help # View detailed info about the chrony in it's interactive mode
  sources # View the info about the current sources
  quit # Exit the chrony interactive mode
```

### 4) Check Servers Connectivity
- Create the shell script:

```sh
    vi ping-script.sh
```

```sh
#!/bin/bash
# The script pings a remote host and shows the output

ping -c1 192.168.1.1 # Update the ip_address to 192.168.1.235 or any. The ping command only pings once.
if [ $? -eq 0 ]
then
echo OK
else
echo NOT OK
fi
```

- Change the file permissions to allow it to be executable:

```sh
    chmod a+x ping-script
    ls -ltr # Confirm the file is now executable
```

- Run the shell script:

```sh
    ./ping-script
```

### 5) Network Interface Card (NIC) Information (ethtool)
- Check ip address for the device/ Find NIC information:

```sh
    ip a
    ip addr
    ip addr show
    ifconfig # Works if you have installed net-tools
```

> - Examples of NICs:
>   - `lo` - Loopback device that your computer uses to communicate with itself. Mainly used for diagnostics & troubleshooting, & to connect to servers running on local machine.
>   - `virb0/ Virtual Bridge 0` - Interface used for NAT (Network Address Translation) and sometimes connect to the outside network.

- View the wired Ethernet network interface drivers & hardware settings:

```sh
    sudo -i # Switch to root
    ethtool interface_name
```

#### NIC/ Network Bonding Procedure
> - Refers to the combination of multiple NIC into a single bond interface for high availability & redundancy.
> - `N\B:` Create a snapshot to revert to the machine snapchot without the NIC Bonding settings

- Get the configurations of your driver/ Install the bonding driver:

```sh
    modprobe bonding
```

- Get the configurations of your bonding:

```sh
    modinfo bonding
```

- Create the bond interface file:

    ```sh
        vi /etc/sysconfig/network-scripts/ifcfg-bond0
    ```

    - Add the following parameters:

     ```sh
         DEVICE=bond0
         TYPE=Bond
         NAME=bond0
         BONDING_MASTER=yes
         BOOTPROTO=none # none/ static is if you want to assign a static IP address
         ONBOOT=yes # Enable when system reboots 
         IPADDR= # Write an ip address that hasn't been taken
         NETMASK=255.255.255.0
         GATEWAY= # IP Address of your modem/ router
         BONDING_OPTS="mode=5 miimon=100"
     ```

- Edit the 1st NIC file (enp0s3):

    ```sh
        vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
    ```

    - Add the following parameters:

     ```sh
         TYPE=Ethernet
         BOOTPROTO=none
         DEVICE=enp0s3
         ONBOOT=yes
         HWADDR="MAC from the ifconfig command"
         MASTER=bond0
         SLAVE=yes
     ```

- Edit the 2nd NIC file (enp0s8):

    ```sh
        vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
    ```

    - Add the following parameters:

     ```sh
         TYPE=Ethernet
         BOOTPROTO=none
         DEVICE=enp0s8
         ONBOOT=yes
         HWADDR="MAC from the ifconfig command"
         MASTER=bond0
         SLAVE=yes
     ```

- Restart the `network` service:
 
```sh
    sudo systemctl restart network
```

- View the bond interface settings like bonding mode & slave interface:

```sh
    cat /proc/net/bonding/bond0
```

### 6) NetworkManager
> - Service providing set of tools designed specifically to make it easier to manage Linux networking configuration & is the default network management service on RHEL 8 & 9.
>   
> - `Network configuration methods`
>     - `i) Network Manager Command Line Interface (nmcli)` 
>     - `ii) Network Manager Text User Interface (nmtui)` 
>     - `iii) nm-connection-editor`
>     - `iv) GNOME Settings`

#### Network Manager Text User Interface (nmtui)
- Start it up:

```sh
    nmtui
```

- `N/B:` Use `Tab` key to navigate the network manager.

#### Network Manager Command Line Interface (nmcli)
- Start it up:

```sh
    nmcli
```

- View the available connections:

```sh
    nmcli con show
```

- Get the listing of network interfaces:

```sh
    nmcli device
```

- Configure a static IP:

    ```sh
        nmcli connection modify interface ipv4.addresses 10.253.1.211/24 # Assign the IP address
        nmcli connection modify interface ipv4.gateway 10.253.1.1 # Assign the gateway
        nmcli connection modify interface ipv4.method manual 
        nmcli connection  modify interface ipv4.dns 8.8.8.8 # DNS
        nmcli connection down interface && nmcli connection up interface # Restarting the network interface
        ip address show interface
    ```
    - `N/B:` Make sure the ip address is not pingable before assigning it to the interface.

- Adding a secondary static IP:

```sh
    nmcli device status # Check the interface connection status
    nmcli connection show --active # View the active interface
    ifconfig
    nmcli connection modify enp0s3 +ipv4.addresses 10.0.0.211/24 # Add the secondary IP address
    nmcli connection reload # Restart the network manager
    systemctl reboot # Reboot the system for the secondary IP address to be added to the interface
    ip address show
```

### 7) SSH & Telnet
> - `SSH` refers to secure shell.
> - Its service daemon is `sshd` and its port number is `22`.

### Configure & secure SSH
#### i) Idle Timeout Interval
- Set the Idle timeout interval:
  
    ```sh
        sudo -i
        cp /etc/ssh/sshd_config /etc/ssh/sshd_config-orig # Backup the file
        vi /etc/ssh/sshd_config
    ```
    
    - Add the following lines in `sshd_config`:
    
    ```sh
        ClientAliveInterval 600 # Time in seconds
        ClientAliveCountMax 0
    ```

- Restart the ssh service:

```sh
    systemctl restart sshd
```

#### ii) Disable root login
- Set the disable root login:

    ```sh
        sudo -i
        vi /etc/ssh/sshd_config
    ```

    - Replace the `PermitRootLogin` yes to no:

    ```sh
        PermitRootLogin no
    ```

- Restart the ssh service:

```sh
    systemctl restart sshd
```

#### iii) Disable Empty Passwords
> - Prevent remote logins from accounts with empty passwords for added security.

- Set the disable empty passwords:

    ```sh
        sudo -i
        vi /etc/ssh/sshd_config
    ```

    - Add this line:

    ```sh
        PermitEmptyPasswords no
    ```

- Restart the ssh service:

```sh
    systemctl restart sshd
```

#### iv) Limits Users' SSH Access
- Set the limit of SSH logins:

    ```sh
        sudo -i
        vi /etc/ssh/sshd_config
    ```

    - Add the following line:

    ```sh
        AllowUsers user1 user 2
    ```

- Restart the ssh service:

```sh
    systemctl restart sshd
```

#### v) Use a different port
> - By default SSH port runs on port `22` hence changing the SSH port number makes the system more secure.

- Change the port number:
  
    ```sh
        sudo -i
        vi /etc/ssh/sshd_config
    ```

    - Change the port number from 22:

    ```sh
        Port 22
    ```

- Restart the ssh service:

```sh
    systemctl restart sshd
```

#### vi) SSH Keys - Access Remote Server without Password
> - Benefits of SSH Keys:
>    1) Repetitive logins
>    2) Automation through scripts
>       
> - Keys are generated at user level in the client and the keys are copied over to the server for seamless login.

##### Steps of Allowing Remote Server Access
- Generate the key:

```sh
    ssh-keygen
```

- Copy the key to the server:

```sh
    ssh-copy-id user@ip_address
```

- Login from client to the server:

```sh
    ssh user@ip_address
```

### 8) SS Command
> - Tool that checks how your system is talking to the internet by displaying open connections, finding and fixing problems.
> - Moreover, it displays the source & destination IP addresses, shows port numbers, & indicates connection states (established, listening etc).
>   
> - SS command shows various types of sockets:
>   i) `Transmission Control Protocol (TCP)` - Set of rules ensuring data is sent successfully between two computers over the internet. Used for web browsing, email, SSH, & file transfers.
>   ii) `User Datagram Protocol (UDP)` - Way of sending data over the internet without checking if it arrives correctly. Doesn't wait for acknowledgements hence faster but less reliable. Used for live streaming.
>   iii) `UNIX` - Way for programs on the same computer to talk to each other by using a special file for message exchange. Used in databases, web servers & local system services without internet.

- Open ss:

    ```sh
        ss
    ```

    - Output columns and meanings:

    ```sh
       Netid - Shows the protocol/ socket being used i.e. UNIX will look like this u_
       State - Shows the status of the connection i.e. ESTAB for Established.
       Recv-Q - Shows the data amount queued for receiving.
       Send-Q - Shows the data amount queued for sending.
       Local Address:Port - Shows the IP and port on your local machine.
       Peer Address:Port - Shows the IP and port on the remote device.
       Process - Identifies which process is using the connection.
    ```

- View only the active tcp connections on the system:

```sh
    ss -t
```

- View only the active udp connections on the system:

```sh
    ss -u
```

- View only the active unix connections on the system:

```sh
    ss -x
```

- View only the connections waiting for incoming requests from other systems:

```sh
    ss -l
```

- View only the active tcp connections that are listening on the system with the ip addresses in numerical form:

```sh
    ss -t -l -n
```

### 9) File Transfer Protocol (FTP)
> - Used for transfer of files between a client and server on a computer network.
> - Built on a client-server model architecture using separate control & data connections between the client and server.
> - Default port is `21`.

#### i) Remote Server
- Install & configure FTP on the remote server:

    ```sh
        sudo -i # Switch to root
        rpm -qa | grep vsftpd # Check if server vsftpd package has been installed
        yum install vsftpd # Install the server vsftpd package if uninstalled
        ping www.google.com # Confirm you have internet connectivity
        cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd-orig.conf # Backup the config file
        vi /etc/vsftpd/vsftpd.conf # Edit the config file
    ```

    - Make the following changes in the file:

    ```sh
        ## Disable anonymous login
        anonymous_enable=NO

        ## Uncomment
        ascii_upload_enable=YES
        ascii_download_enable=YES

        ## Uncomment - Enter your Welcome message
        ftpd_banner=Welcome to blah FTP service.

        ## Add at the end of this file
        use_localtime=YES
    ```

- Configure the service:

```sh
    systemctl start vsftpd
    systemctl enable vsftpd
    systemctl stop firewalld
    systemctl disable firewalld
```

#### ii) Client Server
- Install & configure FTP on the client server:

    ```sh
        sudo -i # Switch to root
        rpm -qa | grep ftp # Check if client ftp package has been installed
        yum install ftp # Install the client ftp package if uninstalled
        ping www.google.com # Confirm you have internet connectivity
        su - user
        touch file1
    ```

    - Transfer the file to the FTP server:

    ```sh
        ftp server_ip_address # Enter the username & password
        bin # Switch to Binary mode because you want to transfer a file
        hash # Show the file transfer progress
        put file1 # Add the file to be transferred
        bye # Exit the ftp interactive cmd 
    ```

    - Pull a file from the FTP server:

    ```sh
        ftp server_ip_address # Enter the username & password
        bin # Switch to Binary mode because you want to transfer a file
        hash # Show the file transfer progress
        get file1 # Pull the file to be transferred
        bye # Exit the ftp interactive cmd 
    ```

### 10) Secure Copy Protocol (SCP)
> - Helps to transfer computer files securely from a local to a remote host.
> - `N/B:` Only difference with FTP is that it adds security and authentication. Default port is `22`.

- Transfer the file to the remote server:

```sh
    su - user
    touch file1
    scp file1 user@ip_address:/file_path
```

### 11) Remote Synchronization (rsync)
> - Transfers & synchronizes files within the same/ remote computer by comparing the modification times & file sizes.
> - It's a lot faster than ftp/ scp and is mostly used to backup files & directories from one server to another. Default port is `22`.

- Syntax of rsync:

```sh
    rsync options source destination
```

- Install rsync in your machine:

```sh
    rpm -qa | grep rsync # Confirm if it has been installed
    yum install rsync
```

- Transfer a file on your local machine:

```sh
    zip -r home_backup.zip /home/user
    mkdir /tmp/backups
    rsync -zvh home_backup.zip /tmp/backups/ 
```

- Transfer a directory on a local machine:

```sh
    rsync -azvh /home/user /tmp/backups
```

- Transfer a file to a remote machine:

```sh
    mkdir /tmp/backups
    rsync -azv file_name user@ip_address:/file_path
```

- Transfer a file from a remote machine:

```sh
    touch file_name
    rsync -azvh user@ip_address:/file_path /destination_file_path
```

### 12) Download Files/ Apps (wget)
> - `wget` stands for worldwide web get.
> - Most use cases are for situations where some programs are not available in your package installer i.e. yum etc hence the need to download from the program's official site.

- Syntax:

```sh
    wget http://website.com/filename
```

### 13) Package Management
> - Package managers include `dnf (for new RHEL versions i.e. 8+)/ yum` for RHEL distros, `apt` for Debian distros. The config file for RHEL is `/etc/yum.repos.d`.
> - `redhat package manager (rpm)` is used when you already have a package downloaded in your system and then you can install it locally. Mostly used in environments without internet access.
> - However, `dnf` downloads and installs the package.

- `dnf` syntax:

```sh
    dnf install -y package_name # Install a package
    dnf localinstall -y package_name # Install a local .rpm package
    dnf remove package_name # Remove a package
```

- `rpm` syntax:

```sh
    rpm -qa # View all the installed packages
    rpm -qa | grep package_name # View for only a specific package
    rpm -qa | wc -l # Get the total number of installed packages
    rpm -ihv /downloaded_package_file_path # Install a downloaded package
    rpm -e package_name # Remove an installed package
```

- Installing a package using a `.rpm` file:

```sh
    uname -m # Confirm the architecture before choosing the .rpm file to download
    wget https://package_download_link/package.rpm
    ls -ltrh # Confirm the file has been downloaded
    rpm -ivh package.rpm # Install the package
    rpm -qa | grep package_name # Confirm the package has been installed
```

- View the information about an installed package:

```sh
    rpm -qi full_package_name
```

- List the configuration files of an installed package:

```sh
    rpm -qc full_package_name 
```

- View which package a command belongs to:

```sh
    which package_name # View the full path for the package
    rpm -qf full_package_path
```

### 14) System Upgrade and Patch Management
> - `Types of upgrades:`
>   - `Major version upgrade` - One version to another higher version e.g. RHEL 7 to 8 or 8 to 9.
>   - `Minor version upgrade` - Makes use of traditional point releases e.g. RHEL 8.1, 8.2 etc.
>     
> - `N/B:` The major version upgrade cannot be upgraded through the dnf command unlike the minor version upgrade.
> - For a major version upgrade, you would have to backup the server, build a new server from scratch and transfer the files.

- View the current running Linux version:

```sh
    cat /etc/os-release
    cat /etc/redhat-release # For RHEL
```

- Perform a minor version upgrade:

```sh
    dnf update -y # For every question, use yes as the answer
```

> - `N/B:` `dnf upgrade` remove the old installed package versions and replaces them with newer ones. `dnf update` preserves the older package versions.

- Rollback a package/ patch:

```sh
    yum install package_name
    yum history # View the history of the yum commands executed  
    yum history undo task_id # Undo the task done
```

- Rollback an update:

```sh
    yum update # Updates and preserves the old packages
    yum upgrade # Upgrade will delete obsolete packages
    yum history undo task_id
```

> - `N/B:` Downgrading a system to a minor version i.e. RHEL 7.1 to RHEL 7.0 isn't recommended as it might leave the system unstable.

### 15) Create a Local Repository (Yum Server)
> - `Repositroy` refers to where all the packages are stored and downloaded from.
> - `/etc/yum.repos.d` is the directory that contains all the files which show Linux all the different mirrors.

- Install the `create_repo` package:

```sh
    dnf install createrepo_c
```

- Mount your DVD/ ISO file in your Linux machine:

```sh
    mkdir local_repo # Create in `/`
    sudo mount /dev/cdrom/ local_repo # Choose any directory you want to mount the disk 
```

- Copy all the packages into the local repo:

```sh
    cd mounted_disk_directory/AppStream/Packages
    ls -ltr | wc -l # Confirm the number of packages
    du -sh . # Confirm you have enough space
    df -kh # Confirm you have enough space in `/`
    cp -rv mounted_disk_directory/AppStream/Packages/* /local_repo/
```

- Remove all the files in the `/etc/yum.repos.d` directory:

```sh
    cd /etc/yum.repos.d
    rm -rf /etc/yum.repos.d/*
```

- Create a new file in the `/etc/yum.repos.d` directory:

```sh
    vi local.repo

    # Add these values
    [CentOS9]
    name=CentOS9
    baseurl=file:///local_repo/
    enabled=1
    gpgcheck=0
```

- Run the `createrepo` command:

```sh
    createrepo_c /local_repo/ # For RHEL versions 8 and above
    createrepo /local_repo/ # For RHEL versions 7 and below 
```

- Clear out any cache for the old repository:

```sh
    dnf clean all
```

- View the details of the current repository:

```sh
    dnf repolist all
```

- Try installing a package using the local repository:

```sh
    dnf install package_name 
```

### 16) Domain Name System (DNS)
> - System that is used to translate a host/ computer name to an IP address.
> - `Types of DNS Records:`
>     - `Pointer Record (PTR)` refers to the record that translates an IP address to the hostname.
>     - `A Record (Address Record)` - refers to the record that translates a hostname to the IP address.
>     - `CNAME Record` - refers to the record that translates a hostname to another hostname.
>
> - `DNS Files & Directories:`
>     - `/etc/named.conf` is the DNS configuration file. `Named` is the process of the DNS. `Bind` is the package that you install for DNS.
>     - `/var/named` is the directory that has all zone files where you define all hostname to IP and vice versa.
> 
> - `Master server` refers to the primary server that stores the master copy of DNS records.
> - `Secondary/ Slave server` refers to a replication to the master server.
> - `Client` refers to any machine that will resolve its hostname/ server's hostname to IP address.
>
> - The default DNS port number is `53`.

#### Setting up a DNS Server (Master Server)
- Install the DNSP package `bind`:

```sh
    sudo dnf install bind bind-utils -y
```

- Modify the `/etc/named.conf`:

```sh
    vi /etc/named.conf

    # Replace with this line
    listen-on port 53 { 127.0.0.1; network_interface_ip; };

    # Go to the bottom of the file and edit/ add these lines at the zone section
    zone "lab.local" IN {
            type master;
            file "forward.lab";
            allow-update { none; };
    };

    ## IP Address written in reverse order
    zone "1.168.192.in-addr.arpa" IN {
            type master;
            file "reverse.lab";
            allow-update { none; };
    };
```

- Create two zone files:

```sh
    cd /var/named
    touch forward.lab # Performs hostname to IP address lookup
    touch reverse.lab # Performs a reverse lookup (IP address to hostname)
```

- Modify the zone files:

```sh
    vi forward.lab

    # Add these lines
    $TTL 86400
    @ IN SOA masterdns.lab.local. root.lab.local. (
    	2011071001 ;Serial
    	3600 ;Refresh
    	1800 ;Retry
    	604800 ;Expire
    	86400 ;Minimum TTL
    )
    @ 		        IN NS 	     masterdns.lab.local.
    @ 		        IN A 	     192.168.100.153
    masterdns       IN A 	     192.168.100.153
    clienta 	    IN A 	     192.168.1.240
    clientb 	    IN A 	     192.168.1.241
```

```sh
    vi reverse.lab

    # Add these lines
    $TTL 86400
    @ IN SOA masterdns.lab.local. root.lab.local. (
    	2011071001 ;Serial
    	3600 ;Refresh
    	1800 ;Retry
    	604800 ;Expire
    	86400 ;Minimum TTL
    )
    @ 		    IN NS 	masterdns.lab.local.
    @ 		    IN PTR 	lab.local.
    masterdns 	IN A 	192.168.100.153
    29 		    IN PTR 	masterdns.lab.local.
    240 		IN PTR 	clienta.lab.local.
    241 		IN PTR 	clientb.lab.local.
```

- Start & enable the DNS service:

```sh
    systemctl start named
    systemctl enable named
```

- Disable the firewall:

```sh
    systemctl stop firewalld
```

- Configure the permissions especially if you run SELinux:

```sh
    chgrp named -R /var/named
    chown -V root:named /etc/named.conf
    restorecon -rv /var/named
    restorecon /etc/named.conf
```

- Verify that the changes made don't have any syntax errors:

```sh
    named-checkconf /etc/named.conf
    named-checkzone lab.local /var/named/forward.lab
    named-checkzone lab.local /var/named/reverse.lab
```

- Modify the network interface config file:

```sh
    cd /etc/NetworkManager/system-connections/
    vi enp0s3.nmconnection

    # Add these line under ipv4 section
    DNS=192.168.100.153
```

- Restart the NetworkManager service:

```sh
    systemctl restart NetworkManager
```

- Modify the configuration file:

```sh
    vi /etc/resolv.conf

    # Edit the nameserver to your new DNS server IP Address
    nameserver 192.168.100.153
```

- Test DNS server using forward lookup:

```sh
    dig masterdns.lab.local # Gives more info than nslookup
    nslookup masterdns.lab.local
    nslookup clienta.lab.local
    nslookup clientb.lab.local
```

- Test DNS server using reverse lookup:

```sh
    dig masterdns.lab.local 
    nslookup masterdns.lab.local
    nslookup 192.168.100.240
    nslookup 192.168.100.241
```

- Add a new entry to the DNS server:

```sh
    cd /var/named
    vi forward.lab
```

```sh
    vi forward.lab

    # Add these lines
    $TTL 86400
    @ IN SOA masterdns.lab.local. root.lab.local. (
    	2011071002 ;Serial
    	3600 ;Refresh
    	1800 ;Retry
    	604800 ;Expire
    	86400 ;Minimum TTL
    )
    @ 		        IN NS 	     masterdns.lab.local.
    @ 		        IN A 	     192.168.100.153
    masterdns       IN A 	     192.168.100.153
    clienta 	    IN A 	     192.168.1.240
    clientb 	    IN A 	     192.168.1.241
    new_client      IN A         ip_address
```

> - `N/B:` Don't forget to increase the serial number by 1.

- Restart the DNS/ named service:

```sh
    systemctl restart named
```

### 17) Mail Servers
> - Computer system that sends and receives emails.
> - Purpose is to handle the storage, processing, and delivery of emails.
> - Emaples of mail servers:
>   - i) Postfix (RHEL/ Centos)
>   - ii) Sendmail (RHEL/ Centos) 
>   - iii) Exim mail server 
>   - iv) Qmail 
>   - v) OpenSMTPD 

#### Postfix
> - Popular, easy to configure, excellent performance, and secure MTA known for it's simplicity and active community support. Hence, making it the preferred mail server for RHEL/ Centos 8+.
> - The configuration files are located in `/etc/postfix/` directory. The main configuration file is `/etc/postfix/main.cf`.

- Install the needed packages `postfix` and `s-nail`:

```sh
  rpm -qa | grep postfix
  dnf install postfix # Install postfix if not installed
  rpm -qa | grep s-nail
  dnf install s-nail # Install s-nail if not installed
```

> - Postfix handles the delivery of the emails on the server (Acts as the post office). S-nail is used to write and send emails on the terminal (Acts as the mail man).
> - The mail relay server is defined in the `main.cf` file. It accepts emails from any server and delivers them to the recipients (Acts as the middleman).

- Configuring Postfix:

  - Edit the configuration file:
    
  ```sh
    vi /etc/postfix/main.cf
  
    # Find the lines starting with relay_hosts and uncomment the parameters fitting your setup
    #relayhost = $mydomain
    #relayhost = [gateway.my.domain]
    #relayhost = [mailserver.isp.tld]
    #relayhost = uucphost
    #relayhost = [an.ip.add.ress]
  ```

  - Restart the service:

  ```sh
    systemctl restart postfix
    systemctl status postfix
  ```

- Sending an email on the terminal:

  ```sh
    mail -s "mail setup" email_address 
  ```

  - Save the email: `Ctrl` + `d`.
  - For the next prompt:
    - Type `yes` to send the email.
    - Type `no` to cancel/ edit the email.

- Dealing with the `postfix` service:

```sh
  systemctl restart postfix
  systemctl start postfix
  systemctl status postfix
  systemctl stop postfix
```
