# Linux-basics
Project by School 21. Linux system installation and updates. Administration basics.

## Part 1. Installation of the OS 
- Checking Ubuntu version:
    ![Installation of the OS 1](/Attachments/1.1.png)


## Part 2. Creating a user
- Creating a user:

        sudo useradd ladrian
        sudo usermod -aG adm ladrian

- The new user is in the output of:

        cat /etc/passwd

    ![Creating a user 1](/Attachments/2.1.png)

## Part 3. Setting up the OS network
- Set the machine name as justicer-1:

        sudo vim /etc/hostname
        reboot

- Set the time zone corresponding to your current location:

        sudo timedatectl set-timezone Europe/Moscow
        sudo timedatectl

    ![setting the time zone](/Attachments/3.2.png)
- Output the names of the network interfaces using a console command:

    ![interfaces](/Attachments/3.3.png)
    - `lo` - software loopback network interface - is used for debugging network programs and running server applications on the local machine. It's a virtual network interface that your computer uses to communicate with itself.
- `DHCP` - Dynamic Host Configuration Protocol
- External ip address of the gateway (ip):

    ![External ip](/Attachments/3.5.png)
- Internal IP address of the gateway, aka default ip address (gw):

    ![Internal IP](/Attachments/3.4.png)
- Set static ip, gw, dns settings (use public DNS servers, e.g. 1.1.1.1 or 8.8.8.8):

        sudo vim /etc/netplan/00-installer-config.yaml
    ![Set static ip, gw, dns settings](/Attachments/3.7.png)
- Ping 1.1.1.1 and ya.ru remote hosts:
    - ![Ping 1.1.1.1](/Attachments/3.8.png)
    - ![Ping ya.ru](/Attachments/3.9.png)

## Part 4. OS Update
- System upgrade:
    ![Ping 1.1.1.1](/Attachments/4.1.png)

## Part 5. Using the sudo command
- `sudo` is a command-line program that allows trusted users to execute commands as root or another user.
- Allow user created in Part 2 to execute sudo command:

        sudo usermod -aG sudo ladrian
        su ladrian

    ![sudo](/Attachments/5.2.png)

## Part 6. Installing and configuring the time service
-     sudo timedatectl set-ntp-on  

    ![timedate](/Attachments/6.1.png)

## Part 7. Installing and using text editors

```
sudo apt install vim
sudo apt install nano
sudo apt install mcedit
```

### Vim
        vim test_vim.txt
- Exit with the saved changes:

    ![vim1](/Attachments/vim1.png)
    
        :wq - quit and save changes
- Exit without saving:

    ![vim2](/Attachments/vim2.png)
    
        :q! - quit without saving
- Search:

    ![vim3](/Attachments/vim_replace.png)
    
        :/expression - searching for expression in file
- Replace:

    ![vim4](/Attachments/vim3.png)
    
        :s/one/two - replacing the first occurrence of one with two

### Nano
        vim test_NANO.txt
- Exit with the saved changes:

    ![nano1](/Attachments/nano1.png)
    
        ctrl + o  - save changes
        ctrl + q - quit
- Exit without saving:

    ![nano2](/Attachments/nano2.png)
    
        ctrl + q - quit
- Search and replace:

    - ![nano3](/Attachments/nano3.png)
    - ![nano4](/Attachments/nano4.png)
    
        ctrl + w - search
        ctrl + / - replace

### McEdit
        vim test_MCEDIT.txt
- Exit with the saved changes:

    ![mcedit1](/Attachments/mcedit1.png)
    
        f2  - save changes
        f10 - quit
- Exit without saving:

    ![mcedit2](/Attachments/mcedit2.png)
    
        f10 - quit
- Search and replace:

    - ![mcedit3](/Attachments/mcedit3.png)
    - ![mcedit4](/Attachments/mcedit4.png)
    
        f7 - search
        f4 - replace

## Part 8. Installing and basic setup of the SSHD service

- Install the SSHd service:

        sudo apt-get install ssh
        sudo apt-get install openssh-server
- Add an auto-start of the service whenever the system boots:

        sudo systemctl enable ssh
- Reset the SSHd service to port `2022`:

        sudo vim /etc/ssh/sshd_config
    ![vim sshd_config](/Attachments/8.1.png)

        sudo service ssh restart
        netstat -tulpan
    - -t - tcp protocol output;
    - -u - udp
    - -l - only listening sockets
    - -p - show the PID and name of the program to which each socket belongs;
    - -a - show both listening and not-listening sockets;
    - -n - show numerical addresses instead of trying to determine symbolic host, port or user names.  
    

    ![netstat](/Attachments/8.2.png)

    - Proto - the protocol used by a socket (tcp, udp, raw);
    - Recv-Q - the count of bytes not copied by the user program connected to this socket;
    - Send-Q - the count of bytes not acknowledged by the remote host;
    - Local Address - address and port number of the local end of the socket;
    - Foreign Address - address and port number of the remote end of the socket;
    - State - the state of the socket;
    - PID/Program name - show the PID and name of the program to which each socket belongs;
    - 0.0.0.0 - IP-address on the local machine.

- Show the presence of the sshd process using the `ps` command:
        
        ps -aux | grep sshd
    ![netstat](/Attachments/8.3.png)

    - ps - report process status;
    - -a - write information for all processes associated with terminals;
    - -u - write information for processes whose user ID numbers or login names are  given  in userlist;
    - -x - list all processes owned by you (same EUID as ps), or to list all processes when used together with the a option.

## Part 9. Installing and using the top, htop utilities

- `top`:
    - uptime: 30 min
    - number of authorised users: 1
    - total system load: 0.00, 0.00, 0.00
    - total number of processes: 94
    - cpu load(percentage): 
        - us, user: time running un-niced user processes; 
        - sy, system: time running kernel processes; 
        - ni, nice: time running niced user processes; 
        - id, idle: time spent in the kernel idle handler; 
        - wa, IO-wait: time waiting for I/O completion; 
        - hi: time spent servicing hardware interrupts; 
        - si: time spent servicing software interrupts; 
        - st: time stolen from this vm by the hypervisor.
    - memory load (KiB):
        - 1983.4 total;
        - 1421.5 free;
        - 147.4 used;
        - 414.5 buff/cache.
    
    ![netstat](/Attachments/9.1.png)
    - pid of the process with the highest memory usage: 666 (top -o %MEM):

        ![netstat](/Attachments/extra3.png)
    - pid of the process taking the most CPU time: 1135 (top -o %CPU):

        ![netstat](/Attachments/extra2.png)

- `htop`:
    - sorted by PID:

    ![netstat](/Attachments/9.2.png)
    - sorted by PERCENT_CPU:

    ![netstat](/Attachments/9.3.png)
    - sorted by PERCENT_MEM:

    ![netstat](/Attachments/9.4.png)
    - sorted by TIME:

    ![netstat](/Attachments/9.5.png)
    - filtered for sshd process:

    ![netstat](/Attachments/9.6.png)
    - with the syslog found:

    ![netstat](/Attachments/9.7.png)
    - with hostname, clock, uptime output added:

    ![netstat](/Attachments/9.8.png)

## Part 10. Using the fdisk utility
-         fdisk -l
    ![netstat](/Attachments/10.1.png)
    - Hard disk name: VBOX HARDDISK
    - Capacity: 20 GiB
    - Number of sectors: 41943040
    - Swap size: 
    ![netstat](/Attachments/10.2.png)

## Part 11. Using the df utility
-       df
    ![netstat](/Attachments/11.1.png)
    
    - `for the root partition (/):`
        - partition size: 10218772
        - space used: 4776008
        - space free: 4902092
        - percentage used: 50%
    the measurement unit: 1K-block = 1024 bytes

-       df -Th
    ![netstat](/Attachments/11.2.png)

    - `for the root partition (/):`
        - partition size: 9.8G
        - space used: 4.6G
        - space free: 4.7G
        - percentage used: 50%
    the file system type: ext4

## Part 12. Using the du utility
- Output the size of the /home, /var, /var/log folders (in bytes, in human readable format):
    - ![netstat](/Attachments/12.1.png)
- Output the size of all contents in /var/log:
    - ![netstat](/Attachments/12.2.png)

## Part 13. Installing and using the ncdu utility
       sudo apt-get install ncdu

- Output the size of the /home:
    - ![netstat](/Attachments/13.1.png)
- Output the size of the /var:
    - ![netstat](/Attachments/13.3.png)
- Output the size of the /var/log:
    - ![netstat](/Attachments/13.4.png)

## Part 14. Working with system logs
-       sudo vim /var/log/auth.log
    Last successful login time, user name and login method:
    ![netstat](/Attachments/14.1.png)
    (09:20:16, justicer by LOGIN)

-       sudo systemctl restart ssh
        sudo vim /var/log/syslog
    ![netstat](/Attachments/14.2.png)

## Part 15. Using the CRON job scheduler
- Using the job scheduler, run the uptime command in every 2 minutes:

        crontab -e
    ![netstat](/Attachments/15.1.png)
- System logs:

        sudo vim /var/log/syslog
    ![netstat](/Attachments/15.3.png)
- List of current jobs for CRON:
        
        crontab -l
    ![netstat](/Attachments/15.2.png)
- Remove all tasks from the job scheduler:
    ![netstat](/Attachments/extra1.png)
