# Day 02: Basic Navigation
## Goal:
* Find the documentation for the commands we used so far
* Navigate between directories, then create, list, move and delete files

## Commands I used: command, what it does, one example
* pwd -> (Print Working Directory) tells us the exact directory we are currently inside.

* cd -> (Change Directory) used to move from one directory to another. {cd ~, cd -, cd ..}
* ls -> (list) used to display the files and directories inside a directory. {ls -l, ls -lh, ls -a}
* mkdir -> (Make Directory) used to create new directories (folders).
* touch -> used to create an empty file or update the timestamps of an existing file.
* mv -> (Move) used for move or rename file/directories.
* cp -> (Copy) used to copy files and directories from one location to another.
* rm -> (Remove) used to delete files and directories from the filesystem.
* rmdir -> (Remove directory) used to remove empty directories.
* man -> (Manual) it lets us read the official docs for commands, system calls, configs, and other linux components.
* apropos -> used to search the descriptions of linux manual (man) pages by keyword.
* tldr -> (Too Long; Didn't Read) provides simplified, practical examples for commands if we don't want to read whole man.

```linux
$ pwd
/home/sarvesh

$ cd Documents/
sarvesh@sarvesh-HP-Laptop-15s-fq5xxx:~/Documents$ 

$ ls
demo         linux-notes                        Study-hacks-by-Neha-patel.pdf
Desktop      Music                              Templates
Documents    mysql-apt-config_0.8.36-1_all.deb  thunderbird
Downloads    Pictures                           Videos

$ mkdir apple
$ ls
apple       install.log                        Public
...

$ touch apple1.py
$ ls
apple1.py

$ mv apple1.py apple2.py
$ ls
apple2.py

$ cp apple2.py apple3.py
$ ls
apple2.py  apple3.py

$ rm apple2.py 
$ ls
apple3.py

$ rmdir apple
...

$ man cp
NAME
       cp - copy files and directories

SYNOPSIS
       cp [OPTION]... [-T] SOURCE DEST
       cp [OPTION]... SOURCE... DIRECTORY
       cp [OPTION]... -t DIRECTORY SOURCE...

DESCRIPTION
       Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.
...

$ apropos network
aseqnet (1)          - ALSA sequencer connectors over network
avahi-autoipd (8)    - IPv4LL network address configuration daemon
bpftool-net (8)      - tool for inspection of networking related bpf prog att...
ctstat (8)           - unified linux network statistics
dhclient-script (8)  - DHCP client network configuration script
dirmngr (8)          - GnuPG's network access daemon
ethtool (8)          - query or control network driver and hardware settings
ifconfig (8)         - configure a network interface
ip (8)               - show / manipulate routing, network devices, interfaces...
ip-link (8)          - network device configuration
...

$ tldr cp
  cp

  Copy files and directories.
  More information: https://www.gnu.org/software/coreutils/manual/html_node/cp-invocation.html.

  - Copy a file to another location:
    cp path/to/source_file path/to/target_file

  - Copy a file into another directory, keeping the filename:
    cp path/to/source_file path/to/target_parent_directory
...
```