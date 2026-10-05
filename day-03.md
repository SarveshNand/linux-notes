# Day 03: Power Trip
## Goal:
* Change the password of your sudo user
* Change the hostname
* Change the timezone


```linux
$ sudo passwd
New password: 
Retype new password: 
passwd: password updated successfully

$ sudo hostname shakira
$ hostname
shakira

$ sudo timedatectl set-timezone America/Sao_Paulo
$ timedatectl
Local time: Wed 2026-10-05 16:38:01 -03
```