# Day 01: Get to know your server
## Goal: 
* Connect and login to your server, preferably using a SSH client
* Run a few simple commands to check the status of your server - like this demo

## Commands I used: command, what it does, one example
* uptime -> shows how long the system is running since its last boot/restart.

* uname -a -> it means Unix Name, displays the info about linux kernel and the system.
* free -h -> used to check RAM(memory) usage on a linux system where -h means human-readable, so it shows Mi, Gi, etc rather than raw bytes.
* lscpu -> used to display detailed info about the CPU/processor and CPU architecture.
* top -> used to see CPU, memory, load, processes and system activity.
* df -h -> used to check filesystem/disk space usage.
* lsblk -> (List Block Devices) used to see the disks, partitions, and other block devices attached to a Linux system, along with their relationships and mount points.
* tree -> used to display directories and files in a tree-like structure.
* du -h -> (Disk Usage) used to find how much disk space files and directories are consuming.
* netstat -i -> used to display network interface stats.
* ip addr -> used to display the network interfaces and IP addresses configured on a Linux system.

```linux
$ uptime
13:29:02 up  3:23,  4 users,  load average: 2.50, 2.40, 3.12

$ uname -a
Linux sarvesh-HP-Laptop-15s-fq5xxx 7.0.0-34-generic #34~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Sep  4 15:38:29 UTC 2 x86_64 x86_64 x86_64 GNU/Linux

$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.4Gi       4.4Gi       2.4Gi       654Mi       1.7Gi       3.0Gi
Swap:          2.0Gi       1.0Gi       986Mi

$ lscpu
Architecture:                x86_64
  CPU op-mode(s):            32-bit, 64-bit
  Address sizes:             39 bits physical, 48 bits virtual
  Byte Order:                Little Endian
CPU(s):                      8
  On-line CPU(s) list:       0-7
...

$ top
top - 13:44:18 up  3:38,  4 users,  load average: 1.87, 1.37, 1.91
Tasks: 319 total,   3 running, 316 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.5 us,  2.5 sy,  0.0 ni, 90.5 id,  1.5 wa,  0.0 hi,  0.0 si,  0.0 st 
MiB Mem :   7609.4 total,   2297.3 free,   4556.1 used,   1710.2 buff/cache     
MiB Swap:   2048.0 total,    986.2 free,   1061.8 used.   3053.3 avail Mem 
...

$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           761M  2.0M  759M   1% /run
efivarfs        256K  154K   98K  62% /sys/firmware/efi/efivars
/dev/nvme0n1p4   98G   49G   45G  53% /
tmpfs           3.8G   75M  3.7G   2% /dev/shm
tmpfs           5.0M   12K  5.0M   1% /run/lock
/dev/nvme0n1p1  256M   78M  179M  31% /boot/efi
tmpfs           761M  200K  761M   1% /run/user/1000

$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0 476.9G  0 disk 
├─nvme0n1p1 259:1    0   260M  0 part /boot/efi
├─nvme0n1p2 259:2    0 375.7G  0 part 
├─nvme0n1p3 259:3    0     1G  0 part 
└─nvme0n1p4 259:4    0   100G  0 part /

$ tree /home
/home
└── sarvesh
    ├── demo
    │   ├── HELP.md
    │   ├── mvnw
    │   ├── mvnw.cmd
    │   ├── pom.xml
    │   ├── src
    │   │   ├── main
    │   │   │   ├── java
    │   │   │   │   └── com
    │   │   │   │       └── example
    │   │   │   │           └── demo
    │   │   │   │               ├── DemoApplication.java
    │   │   │   │               └── HelloController.java
    │   │   │   └── resources
    │   │   │       ├── application.properties
    │   │   │       ├── static
    │   │   │       └── templates
...

$ du -h
4.0K	./Postman/files
8.0K	./Postman
8.0K	./.swt
4.0K	./Videos
4.0K	./Desktop
4.0K	./.thunderbird/Crash Reports/events
8.0K	./.thunderbird/Crash Reports
8.0K	./.thunderbird/c7hlm4mx.default
4.0K	./.thunderbird/Pending Pings
...

$ netstat -i
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
lo              65536   129094      0      0 0        129094      0      0      0 LRU
wlo1             1500  2180554      0      0 0        522288      0      8      0 BMRU

$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
...
```
