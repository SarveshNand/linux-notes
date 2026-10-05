 # Linux File System Architecture

---

 # 1\. The Big Picture

 ## Linux uses ONE unified filesystem tree

 Unlike Windows:

```
Windows
│
├── C:\
├── D:\
├── E:\
└── F:\
```

 Linux uses:

```
                         /
                    ROOT DIRECTORY
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
     /home              /etc              /var
       │                 │                 │
    users            configuration       logs/data
       │
   ┌───┴────┐
   │        │
 alice     bob
```

 ### The key idea

 **Everything starts from `/`.**

 `/` is the **root of the entire filesystem hierarchy**.

 It can contain:

 - Operating-system files
- User files
- Applications
- Configuration
- Logs
- Devices
- Kernel interfaces
- Temporary files
- Mounted disks
- USB drives
- Network filesystems

---

 # 2\. The Most Important Mental Model

 Think of Linux's filesystem as a **single tree**:

```
                              /
                              │
        ┌─────────────────────┼────────────────────────┐
        │                     │                        │
      /home                  /etc                     /var
        │                     │                        │
   User Data             Configuration              Logs/Data
        │                     │                        │
   ┌────┴────┐          ┌─────┼─────┐             /var/log
   │         │          │     │     │
 alice      bob       ssh   nginx  hosts
```

 The important distinction is:

```
/       = Root DIRECTORY
/root   = Home DIRECTORY of the root USER
```

 These are **not the same thing**.

---

 # 3\. `/` vs `/root`

 This is one of the most common Linux beginner mistakes.

```
/
│
├── home/
│   ├── alice/
│   └── bob/
│
├── root/
│   └── files belonging to root user
│
├── etc/
├── var/
├── usr/
└── ...
```

 ### `/`

 The top-level directory of the entire filesystem.

```
/
```

 Think:

 > "Where everything begins."

 ### `/root`

 The home directory of the `root` user.

```
/root
```

 Think:

 > "The administrator's personal home."

 ### Easy memory trick

```
/       → Root of FILESYSTEM
/root   → Home of ROOT USER
```

---

 # 4\. Complete Linux Filesystem Map

 A simplified Linux filesystem looks like this:

```
/
│
├── boot/       → Bootloader + Kernel
│
├── dev/        → Device files
│
├── etc/        → System configuration
│
├── home/       → Regular users' home directories
│
├── root/       → root user's home directory
│
├── run/        → Runtime state
│
├── tmp/        → Temporary files
│
├── var/        → Variable/changing data
│
├── usr/        → Most user-space programs and resources
│
├── bin/        → Essential executables*
│
├── sbin/       → Essential system/admin executables*
│
├── lib/        → Essential shared libraries*
│
├── media/      → Removable-media mount points
│
├── mnt/        → Temporary/manual mount points
│
├── opt/        → Optional/add-on software
│
├── proc/       → Process/kernel virtual filesystem
│
├── sys/        → Kernel/device virtual filesystem
│
└── srv/        → Data served by system services
```

 > `*` On many modern distributions, `/bin`, `/sbin`, and `/lib` are symbolic links into `/usr`. The traditional names remain important because you will encounter them frequently.

---

 # 5\. Functional Classification

 Instead of memorizing 15+ directories individually, group them.

```
LINUX FILESYSTEM
│
├── 1. USER DATA
│   ├── /home
│   └── /root
│
├── 2. SOFTWARE & EXECUTABLES
│   ├── /bin
│   ├── /sbin
│   └── /usr
│
├── 3. CONFIGURATION
│   └── /etc
│
├── 4. LIBRARIES
│   └── /lib
│
├── 5. CHANGING / TEMPORARY DATA
│   ├── /var
│   ├── /run
│   └── /tmp
│
├── 6. KERNEL / HARDWARE INTERFACES
│   ├── /dev
│   ├── /proc
│   └── /sys
│
├── 7. STORAGE MOUNTS
│   ├── /media
│   └── /mnt
│
└── 8. BOOT / OPTIONAL / SERVICE DATA
    ├── /boot
    ├── /opt
    └── /srv
```

 This grouping is much easier to remember.

---

 # 6\. `/home` — Regular Users

 ## Purpose

 `/home` contains the personal directories of normal users.

 Example:

```
/home
│
├── alice/
│   ├── Documents/
│   ├── Downloads/
│   ├── Pictures/
│   └── .config/
│
├── bob/
│   ├── Documents/
│   └── projects/
│
└── john/
```

 For example:

```
/home/alice
```

 is Alice's home directory.

 You can see the current user's home using:

```
echo $HOME
```

 or:

```
pwd
```

 when the user is currently inside their home directory.

 ### DevOps relevance

 You will commonly encounter:

```
/home/<username>/.ssh/
```

 For example:

```
/home/alice/.ssh/
├── id_rsa
├── id_rsa.pub
└── authorized_keys
```

 SSH configuration and keys are extremely important in DevOps.

---

 # 7\. `/root` — Root User's Home

 `root` is the superuser.

 Its home directory is:

```
/root
```

 Notice:

```
/home/alice     → Alice's home
/home/bob       → Bob's home
/root           → root user's home
```

 `/root` is **not** the filesystem root.

 ### Remember

```
/       → filesystem root
/root   → root user's home
```

---

 # 8\. `/bin` — Essential Executables

 Historically, `/bin` contained essential user commands required for basic system operation.

 Examples include:

```
/bin
├── ls
├── cp
├── mv
├── cat
├── mkdir
├── rm
└── ...
```

 You can check:

```
ls /bin
```

 ### Important modern Linux note

 On many modern distributions:

```
/bin → /usr/bin
```

 So `/bin` may actually be a symbolic link.

 Check with:

```
ls -ld /bin
```

 ### DevOps takeaway

 Know `/bin`, but also understand the modern `/usr/bin` layout.

---

 # 9\. `/sbin` — System Administration Executables

 Historically, `/sbin` was used for system-management commands.

 Examples:

```
/sbin
├── ip
├── mount
├── fsck
├── reboot
└── ...
```

 Modern distributions may have:

```
/sbin → /usr/sbin
```

 Check:

```
ls -ld /sbin
```

 ### Mental model

```
/bin
   ↓
Basic commands

/sbin
   ↓
System administration commands
```

---

 # 10\. `/usr` — Major User-Space Software Tree

 This is one of the most important directories for a DevOps engineer.

 Historically, `/usr` means **Unix System Resources**.

 It is **not simply "user files."**

 A typical system contains:

```
/usr
│
├── bin/        → Most user-space executables
├── sbin/       → System administration programs
├── lib/        → Libraries
├── share/      → Architecture-independent data
├── local/      → Locally installed software
└── ...
```

 For example:

```
/usr/bin
├── python
├── git
├── curl
├── ssh
├── grep
├── vim
└── ...
```

 Check:

```
ls /usr/bin
```

 ### Finding where a command comes from

```
which git
```

 or preferably:

```
command -v git
```

 Example:

```
/usr/bin/git
```

---

 # 11\. `/etc` — Configuration

 ## One of the MOST important directories for DevOps

 `/etc` contains system-wide configuration files.

 Think:

```
/etc
   ↓
"How should this machine behave?"
```

 Example:

```
/etc
│
├── hostname
├── hosts
├── fstab
├── passwd
├── group
├── ssh/
├── systemd/
├── nginx/
└── ...
```

 Common examples:

 | File/Directory | Purpose |
| --- | --- |
| `/etc/hostname` | System hostname |
| `/etc/hosts` | Local hostname resolution |
| `/etc/fstab` | Persistent filesystem mount configuration |
| `/etc/passwd` | User account information |
| `/etc/group` | Group information |
| `/etc/ssh/` | SSH server/client configuration |
| `/etc/systemd/` | systemd-related configuration |
| `/etc/nginx/` | Nginx configuration, when installed |

### DevOps mental model

```
Application
     │
     ↓
Configuration
     │
     ↓
    /etc
```

 For example:

```
/etc/nginx/nginx.conf
```

---

 # 12\. `/lib` — Shared Libraries

 Applications frequently depend on shared libraries.

 Think of a library as reusable code.

```
Application A ──┐
                │
Application B ──┼──→ Shared Library
                │
Application C ──┘
```

 Instead of every application carrying its own copy:

```
Program
   ↓
Shared Library
   ↓
Operating System functionality
```

 Historically:

```
/lib
```

 contained essential shared libraries.

 On modern distributions it may be:

```
/lib → /usr/lib
```

 ### Related directories

```
/usr/lib
/usr/lib64
```

 may contain shared libraries depending on distribution and architecture.

---

 # 13\. `/var` — Variable Data

 ## Very important for DevOps and troubleshooting

 `/var` contains data that **changes while the system is running**.

 Think:

```
/var
 ↓
"Things that vary/change over time"
```

 Typical structure:

```
/var
│
├── log/        → Logs
├── cache/      → Cached data
├── lib/        → Persistent application/system state
├── spool/      → Queued/spooled data
└── tmp/        → Temporary files
```

---

 # 14\. `/var/log` — Logs

 This is extremely important.

```
/var/log
│
├── syslog
├── auth.log
├── kern.log
├── nginx/
├── apache2/
└── ...
```

 Exact files depend on the Linux distribution and services installed.

 Useful commands:

```
ls /var/log
```

```
tail -f /var/log/syslog
```

 On systemd-based systems, you will often use:

```
journalctl
```

 ### DevOps troubleshooting pattern

 When something fails:

```
Application problem
       │
       ↓
Check application logs
       │
       ↓
/var/log
       │
       ↓
journalctl
       │
       ↓
Identify error
```

---

 # 15\. `/tmp` — Temporary Files

 `/tmp` is used for temporary files.

```
/tmp
│
├── temporary-file-1
├── application-temp
└── ...
```

 Applications may use it for short-lived data.

 ### Important

 Do **not** assume:

 > "`/tmp` is always completely deleted on every reboot."

 Cleanup behavior depends on the distribution and system configuration.

 On many systemd systems, `/tmp` is cleaned automatically according to configured policies.

 ### DevOps rule

 Do not store important persistent application data in `/tmp`.

```
/tmp
 ↓
Temporary
 ↓
Not reliable for persistent data
```

---

 # 16\. `/run` — Runtime State

 Modern Linux systems commonly use:

```
/run
```

 for **volatile runtime data** created since boot.

 Examples:

```
/run
├── lock/
├── systemd/
├── user/
└── ...
```

 You may encounter:

```
/run/nginx.pid
/run/sshd.pid
```

 depending on the system.

 ### Key distinction

```
/var
 ↓
Persistent variable data

/run
 ↓
Runtime state
 ↓
Usually exists in memory and is recreated at boot
```

---

 # 17\. `/dev` — Device Files

 Linux follows a famous Unix philosophy:

 > **"Everything is a file"**

 Hardware devices are represented through special files under:

```
/dev
```

 Example:

```
/dev
│
├── sda
├── sda1
├── nvme0n1
├── nvme0n1p1
├── null
├── zero
├── random
└── tty
```

 ### Storage example

 A disk may appear as:

```
/dev/sda
```

 with partitions:

```
/dev/sda1
/dev/sda2
```

 An NVMe disk might look like:

```
/dev/nvme0n1
```

 with partitions:

```
/dev/nvme0n1p1
/dev/nvme0n1p2
```

 ### Check devices

```
ls /dev
```

 For block devices:

```
lsblk
```

---

 # 18\. `/proc` — Process & Kernel Information

 `/proc` is **not a normal disk directory**.

 It is a virtual filesystem provided by the Linux kernel.

```
/proc
   ↓
Virtual filesystem
   ↓
Kernel exposes system/process information
```

 Example:

```
/proc
│
├── 1/
├── 2/
├── cpuinfo
├── meminfo
├── loadavg
├── uptime
├── version
└── ...
```

---

 # 19\. `/proc` and Processes

 Each running process generally has a directory identified by its PID.

 Example:

```
/proc
│
├── 1/
├── 100/
├── 250/
├── 1045/
└── ...
```

 If process ID is:

```
1045
```

 you can inspect:

```
ls /proc/1045
```

 This provides information about that process.

 Useful:

```
cat /proc/cpuinfo
```

 CPU information.

```
cat /proc/meminfo
```

 Memory information.

```
cat /proc/uptime
```

 System uptime.

```
cat /proc/loadavg
```

 Load information.

 ### DevOps mental model

```
/proc
   │
   ├── Process information
   ├── CPU information
   ├── Memory information
   ├── Kernel information
   └── Runtime system information
```

---

 # 20\. `/sys` — Kernel & Hardware Information

 Another extremely important virtual filesystem is:

```
/sys
```

 It exposes information about:

 - Devices
- Hardware
- Kernel subsystems
- Drivers
- Device relationships

 Think:

```
/proc
 ↓
Processes + kernel/system information

/sys
 ↓
Devices + hardware + kernel device model
```

 Example:

```
ls /sys
```

 Common areas include:

```
/sys/class
/sys/devices
/sys/block
```

---

 # 21\. `/media` — Removable Media

 Desktop Linux systems commonly use:

```
/media
```

 for automatically mounted removable devices.

 Example:

```
/media
└── alice
    └── USB_DRIVE
```

 A USB drive might therefore become available under a path such as:

```
/media/alice/USB_DRIVE
```

 Exact behavior depends on the desktop environment and distribution.

---

 # 22\. `/mnt` — Manual Mount Point

 `/mnt` is conventionally used for temporary/manual filesystem mounts.

 Example:

```
mount /dev/sdb1 /mnt
```

 Conceptually:

```
/dev/sdb1
    │
    │ mount
    ↓
  /mnt
    │
    └── Filesystem becomes accessible here
```

 This is a very important concept.

---

 # 23\. What Does "Mount" Actually Mean?

 Suppose you have another disk:

```
/dev/sdb1
```

 The disk exists, but its filesystem is not necessarily accessible through the normal directory tree yet.

 You can mount it:

```
mount /dev/sdb1 /mnt/data
```

 Then:

```
/dev/sdb1
     │
     │
     ▼
 /mnt/data
     │
     ├── file1
     ├── file2
     └── projects/
```

 Now the filesystem appears as part of the Linux directory tree.

 ### This is the key idea

 Linux does **not** need:

```
D:
E:
F:
```

 Instead:

```
/
│
├── home/
├── etc/
├── var/
│
└── mnt/
    └── data/
        └── files...
```

---

 # 24\. `/boot` — Boot Files

 `/boot` contains files needed during system startup.

 Typical contents may include:

```
/boot
│
├── vmlinuz-...
├── initramfs-...
├── grub/
└── ...
```

 Important components:

```
Linux Kernel
     +
Initial RAM filesystem
     +
Bootloader configuration
```

---

 # 25\. `/boot` Startup Flow

 A simplified boot process:

```
Power ON
   │
   ↓
Firmware
BIOS / UEFI
   │
   ↓
Bootloader
GRUB
   │
   ↓
Linux Kernel
   │
   ↓
initramfs
   │
   ↓
systemd / init
   │
   ↓
Services
   │
   ↓
Login / Application environment
```

 Relevant files commonly live under:

```
/boot
```

---

 # 26\. `/opt` — Optional Software

 `/opt` is traditionally used for optional/add-on application software.

 Example:

```
/opt
├── application1/
├── application2/
└── vendor-software/
```

 You may encounter vendor applications installed under:

```
/opt/<application>
```

---

 # 27\. `/srv` — Service Data

 `/srv` is intended for data served by system services.

 Example:

```
/srv
├── www/
├── ftp/
└── application-data/
```

 It is less universally encountered than `/etc`, `/var`, `/usr`, etc., but it is part of the standard filesystem hierarchy.

---

 # 28\. `/usr/local` — Locally Installed Software

 A useful DevOps directory:

```
/usr/local
```

 is conventionally used for software installed locally by the administrator rather than managed as part of the operating system's normal package set.

 Typical structure:

```
/usr/local
│
├── bin/
├── sbin/
├── lib/
└── share/
```

 For example:

```
/usr/local/bin/my-script
```

---

 # 29\. Important Directory Relationships

 A very useful way to understand Linux:

```
                         /
                         │
     ┌───────────────────┼─────────────────────┐
     │                   │                     │
   /etc                 /usr                  /var
     │                   │                     │
     │                   │                     ├── log
     │                   │                     ├── cache
     │                   │                     └── lib
     │                   │
     │                   ├── bin
     │                   ├── sbin
     │                   └── lib
     │
     ├── ssh
     ├── systemd
     ├── fstab
     └── hostname
```

 Think:

```
/usr → Software
/etc → Configuration
/var → Changing/Persistent state
```

 This trio is extremely important in DevOps.

---

 # 30\. DevOps Troubleshooting Map

 When troubleshooting a Linux server, think by category.

```
PROBLEM
   │
   ├── Application configuration?
   │       └── /etc
   │
   ├── Logs?
   │       ├── /var/log
   │       └── journalctl
   │
   ├── Application installed where?
   │       ├── /usr/bin
   │       ├── /usr/local/bin
   │       └── /opt
   │
   ├── Disk/device problem?
   │       ├── /dev
   │       ├── lsblk
   │       └── df
   │
   ├── Process problem?
   │       ├── /proc
   │       ├── ps
   │       └── top
   │
   ├── Mount problem?
   │       ├── /etc/fstab
   │       ├── /mnt
   │       └── mount
   │
   └── Boot problem?
           └── /boot
```

---

 # 31\. Essential Commands for Filesystem Exploration

 ## See the root directory

```
ls /
```

 Better:

```
ls -lah /
```

---

 ## Show current directory

```
pwd
```

 Example:

```
/home/alice/projects
```

---

 ## List directory contents

```
ls
```

 Detailed:

```
ls -lah
```

---

 ## Move between directories

```
cd /etc
```

 Go home:

```
cd ~
```

 Go to parent:

```
cd ..
```

 Go to root:

```
cd /
```

---

 # 32\. Finding Files

 Use:

```
find
```

 Example:

```
find /etc -name "nginx.conf"
```

 Find all `.log` files:

```
find /var/log -name "*.log"
```

---

 # 33\. Finding Commands

```
command -v nginx
```

 or:

```
which nginx
```

 For package-managed files, tools such as the distribution's package manager can tell you which package owns a file.

---

 # 34\. Disk Usage

 ## Filesystem-level usage

```
df -h
```

 Example:

```
Filesystem      Size  Used Avail Use%
/dev/sda2       100G   70G   30G  70%
```

 Think:

```
df
 ↓
Disk/filesystem capacity
```

---

 ## Directory-level usage

```
du -sh /var
```

 For subdirectories:

```
du -sh /var/*
```

 Think:

```
du
 ↓
Directory/file space consumption
```

 ### Important DevOps distinction

```
df → How full is the filesystem?

du → What is consuming the space?
```

---

 # 35\. Inspect Block Devices

 Use:

```
lsblk
```

 Example conceptual output:

```
NAME        SIZE TYPE MOUNTPOINTS
sda         100G disk
├─sda1        1G part /boot
└─sda2       99G part /
sdb          50G disk
└─sdb1       50G part /data
```

 This is one of the most useful commands when working with disks.

---

 # 36\. View Mounted Filesystems

 Use:

```
findmnt
```

 or:

```
mount
```

 You can also inspect:

```
cat /proc/mounts
```

---

 # 37\. Persistent Mount Configuration

 One of the most important DevOps files:

```
/etc/fstab
```

 It defines filesystems that should generally be mounted automatically.

 Conceptually:

```
/etc/fstab
     │
     ↓
"Which filesystems should be mounted,
 where, and with which options?"
```

 Example conceptual entry:

```
UUID=xxxx-xxxx   /data   ext4   defaults   0 2
```

 ### DevOps warning

 A mistake in `/etc/fstab` can prevent a system from booting normally.

 Always validate carefully before rebooting.

---

 # 38\. Linux Filesystem Architecture — One Complete Diagram

```
                                      /
                              ROOT FILESYSTEM
                                      │
       ┌───────────────┬──────────────┼───────────────┬──────────────┐
       │               │              │               │              │
     /boot            /etc           /home           /root          /usr
       │               │              │               │              │
 Kernel +            System        Regular          Root-user       Software
 bootloader          Config        users            home            │
                                                                      │
                                                        ┌─────────────┼──────────┐
                                                        │             │          │
                                                       bin           sbin       lib
                                                        │
                                                     Programs
       │
       ├─────────────────────────────────────────────────────────────────────┐
       │                                                                     │
      /var                                                                  /tmp
       │                                                                     │
   Changing data                                                        Temporary data
       │
   ┌───┼────┬───────┐
   │   │    │       │
 log cache lib    spool

       ┌─────────────────────────────────────────────────────────────────────┐
       │                                                                     │
      /dev                                                                  /proc
       │                                                                     │
 Device interfaces                                                    Kernel/process info

       ┌─────────────────────────────────────────────────────────────────────┐
       │                                                                     │
      /sys                                                                /run
       │                                                                     │
 Hardware/kernel                                                       Runtime state

       ┌───────────────────────────────┐
       │                               │
     /media                          /mnt
       │                               │
 Removable media                Manual mounts
```

---

 # 39\. "Where Would I Look?" — DevOps Cheat Sheet

 | Requirement | Look Here |
| --- | --- |
| User's files | `/home` |
| root user's files | `/root` |
| System configuration | `/etc` |
| SSH configuration | `/etc/ssh` |
| Filesystem mount configuration | `/etc/fstab` |
| System hostname | `/etc/hostname` |
| User account database | `/etc/passwd` |
| System logs | `/var/log` |
| Runtime state | `/run` |
| Temporary files | `/tmp` |
| Executable programs | `/usr/bin`, `/bin` |
| Admin programs | `/usr/sbin`, `/sbin` |
| Libraries | `/usr/lib`, `/lib` |
| Kernel boot files | `/boot` |
| Devices | `/dev` |
| Processes/kernel info | `/proc` |
| Hardware/kernel device info | `/sys` |
| Removable media | `/media` |
| Manual mounts | `/mnt` |
| Optional software | `/opt` |
| Locally installed software | `/usr/local` |
| Service data | `/srv` |

---

 # 40\. The 80/20 Directories for DevOps

 You do **not** need to memorize every directory equally.

 Focus heavily on these:

```
        ┌─────────────────────────────────────┐
        │       DEVOPS 80/20 DIRECTORIES      │
        └─────────────────────────────────────┘

/etc
 ↓
Configuration

/var/log
 ↓
Logs

/var
 ↓
Changing application/system data

/usr/bin
 ↓
Programs

/home
 ↓
User data

/root
 ↓
Root user's home

/dev
 ↓
Devices/disks

/proc
 ↓
Processes + kernel information

/sys
 ↓
Hardware + kernel device information

/boot
 ↓
Kernel + bootloader

/tmp
 ↓
Temporary files

/run
 ↓
Runtime state

/etc/fstab
 ↓
Persistent mounts
```

---

 # 41\. Most Important Mental Models

 ## Mental Model #1

```
/ = Everything
```

---

 ## Mental Model #2

```
/home = Users
/root = Root user
```

---

 ## Mental Model #3

```
/etc = Configuration
```

 Remember:

 > **"How should the system/application behave?"**

 Look in `/etc`.

---

 ## Mental Model #4

```
/var = Things that change
```

 Especially:

```
/var/log = Logs
```

---

 ## Mental Model #5

```
/usr = Software
```

 Especially:

```
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

---

 ## Mental Model #6

```
/dev = Devices
```

 Think:

```
/dev/sda
/dev/nvme0n1
/dev/null
```

---

 ## Mental Model #7

```
/proc = Processes + kernel state
/sys  = Hardware + kernel device model
```

---

 ## Mental Model #8

```
/boot = How Linux starts
```

---

 ## Mental Model #9

```
/mnt + /media = Where other filesystems can appear
```

---

 # 42\. Modern Linux: Important `/usr` Merge Concept

 If you are working with modern distributions, remember this:

 Historically:

```
/bin
/sbin
/lib
```

 were separate directories.

 Modern systems commonly use a merged layout:

```
/bin  → /usr/bin
/sbin → /usr/sbin
/lib  → /usr/lib
```

 Therefore, do not be surprised if:

```
ls -ld /bin
```

 shows something like:

```
/bin -> usr/bin
```

 This is normal.

 ### Revision point

 Do not memorize:

 > "`/bin` must physically contain all binaries."

 Instead remember:

 > "`/bin` is the traditional location for essential executables, and on modern systems it may be a symlink into `/usr/bin`."

---

 # 43\. Filesystem Hierarchy — DevOps Perspective

 A useful conceptual pipeline:

```
                 LINUX SERVER
                      │
       ┌──────────────┼──────────────┐
       │              │              │
   SOFTWARE       CONFIGURATION     DATA
       │              │              │
      /usr           /etc           /var
       │              │              │
   binaries        configs          logs
   libraries       services        databases
   tools           networking      queues
```

 Then:

```
HARDWARE
   │
   ↓
 /dev
   │
   ↓
Filesystem
   │
   ↓
 Mount Point
   │
   ↓
Directory Tree
```

 And:

```
KERNEL
  │
  ├── /proc → Process/system information
  │
  └── /sys  → Hardware/device information
```

---

 # 44\. Common DevOps Scenarios

 ## Scenario 1 — Disk is full

 Start with:

```
df -h
```

 Then identify large directories:

```
du -sh /*
```

 Investigate:

```
/var
```

 especially:

```
/var/log
```

 Mental path:

```
Disk Full
   ↓
df -h
   ↓
Find filesystem
   ↓
du -sh
   ↓
Find large directory
   ↓
Investigate logs/data/cache
```

---

 ## Scenario 2 — SSH server configuration problem

 Look at:

```
/etc/ssh/
```

 Common server configuration:

```
/etc/ssh/sshd_config
```

 Then inspect service status/logs.

---

 ## Scenario 3 — Application is running but behaving incorrectly

 Check:

```
/etc/<application>/
```

 for configuration.

 Then:

```
/var/log/
```

 for logs.

 Then:

```
ps aux
```

 or:

```
systemctl status <service>
```

 for runtime state.

---

 ## Scenario 4 — Disk is not visible

 Start with:

```
lsblk
```

 Then:

```
ls /dev
```

 Then inspect mounts:

```
findmnt
```

 Then investigate:

```
/etc/fstab
```

 if the disk should mount automatically.

---

 ## Scenario 5 — Process investigation

 Find the PID:

```
ps aux
```

 Suppose PID is:

```
1234
```

 Then inspect:

```
ls /proc/1234
```

 Useful:

```
cat /proc/1234/status
```

---

 # 45\. Quick Revision Table

 | Directory | Remember It As | DevOps Importance |
| --- | --- | --- |
| `/` | Everything starts here | ⭐⭐⭐⭐⭐ |
| `/etc` | Configuration | ⭐⭐⭐⭐⭐ |
| `/var` | Changing data | ⭐⭐⭐⭐⭐ |
| `/var/log` | Logs | ⭐⭐⭐⭐⭐ |
| `/usr` | Software/resources | ⭐⭐⭐⭐⭐ |
| `/dev` | Devices | ⭐⭐⭐⭐⭐ |
| `/proc` | Processes/kernel | ⭐⭐⭐⭐⭐ |
| `/sys` | Hardware/kernel devices | ⭐⭐⭐⭐ |
| `/home` | User files | ⭐⭐⭐⭐ |
| `/root` | Root user's home | ⭐⭐⭐⭐ |
| `/boot` | Kernel/bootloader | ⭐⭐⭐⭐ |
| `/run` | Runtime state | ⭐⭐⭐⭐ |
| `/tmp` | Temporary data | ⭐⭐⭐ |
| `/mnt` | Manual mounts | ⭐⭐⭐ |
| `/media` | Removable media | ⭐⭐⭐ |
| `/opt` | Optional software | ⭐⭐ |
| `/srv` | Service data | ⭐⭐ |

---

 # 46\. 30-Second Revision

 If you have only 30 seconds before an interview or exam:

```
/
│
├── /home     → Regular users
├── /root     → Root user's home
│
├── /etc      → Configuration
│
├── /usr      → Software & resources
├── /bin      → Essential commands
├── /sbin     → System commands
├── /lib      → Libraries
│
├── /var      → Changing data
│   └── log/  → Logs
│
├── /tmp      → Temporary files
├── /run      → Runtime state
│
├── /dev      → Devices
├── /proc     → Processes/kernel info
├── /sys      → Hardware/kernel info
│
├── /boot     → Kernel + bootloader
│
├── /media    → Removable media
├── /mnt      → Manual mounts
│
├── /opt      → Optional software
└── /srv      → Service data
```

---

 # 47\. Interview-Level Questions

 ### Q1. What is `/`?

 The root directory and top of the Linux filesystem hierarchy.

 ### Q2. Difference between `/` and `/root`?

```
/       → Root of filesystem
/root   → Home directory of root user
```

 ### Q3. Where are Linux configuration files stored?

 Primarily under:

```
/etc
```

 ### Q4. Where are system logs commonly stored?

 Commonly:

```
/var/log
```

 On systemd systems, logs are also accessed through:

```
journalctl
```

 ### Q5. What is `/proc`?

 A virtual filesystem exposing process and kernel/system information.

 ### Q6. What is `/dev`?

 A filesystem containing device nodes representing hardware and pseudo-devices.

 ### Q7. Difference between `/mnt` and `/media`?

 Traditionally:

```
/mnt
  → temporary/manual mounts

/media
  → removable media mounts
```

 Exact behavior depends on the distribution and environment.

 ### Q8. What is `/etc/fstab`?

 Configuration describing filesystems and mount points that can be mounted automatically.

 ### Q9. What is `/var`?

 A location for variable/changing data such as logs, caches, queues, and application state.

 ### Q10. What is `/usr`?

 The major hierarchy containing user-space programs, libraries, documentation, and other resources.

---

 # 48\. Final Memory Formula

 The entire filesystem can be remembered with this:

```
                  LINUX FILESYSTEM

                        /
                        │
       ┌────────────────┼─────────────────┐
       │                │                 │
     USERS          SOFTWARE          CONFIG
       │                │                 │
   /home /root        /usr              /etc
                         │
                  /bin /sbin /lib

       ┌────────────────┼─────────────────┐
       │                │                 │
      DATA           HARDWARE          KERNEL
       │                │                 │
      /var            /dev          /proc + /sys
       │
    /var/log

       ┌────────────────┼─────────────────┐
       │                │                 │
   TEMPORARY          BOOT             MOUNTS
       │                │                 │
      /tmp             /boot        /mnt + /media

                       │
                       │
                    RUNTIME
                       │
                      /run
```

 ## The ultimate DevOps mnemonic

```
/etc   → CONFIG
/usr   → SOFTWARE
/var   → CHANGING DATA
/var/log → LOGS
/home  → USERS
/root  → ROOT USER
/dev   → DEVICES
/proc  → PROCESSES
/sys   → HARDWARE
/boot  → BOOT
/run   → RUNTIME
/tmp   → TEMPORARY
/mnt   → MANUAL MOUNTS
/media → REMOVABLE MEDIA
/opt   → OPTIONAL SOFTWARE
/srv   → SERVICE DATA
```
