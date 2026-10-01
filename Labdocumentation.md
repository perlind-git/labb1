# **Per Lindberg, 27-09-26, ISCXS26, Labbenvironment, Git, CLI och AI**

# Labenvironment: 
Oracle VrtualBox with one Windows 11 and one Ubuntu 25 virtual machines connected to a NAT Network (Labnet) - (host-only malfunctioned in my Oracle setup so I chose NAT)

# Network settings

| Hostname | OS         | IP-Adress    | Netmask       | Gateway     |
|----------|------------|--------------|---------------|-------------|
| Win11    | Windows 11 | 192.168.1.50 | 255.255.255.0 | 192.168.1.1 |
| Ubuntu   | Ubuntu 25  | 192.168.1.51 | 255.255.255.0 | 192.168.1.1 |

# Commands
**Linux**

**Create folder**

>vboxuser@Ubuntu:~$ sudo mkdir /var/systementor

>vboxuser@Ubuntu:~$ sudo mkdir /var/systementor/konsultdata

**Create file**

>sudo touch /var/systementor/konsultdata/anteckningar.txt

**Create group "konsulter"**

>sudo addgroup konsulter

**Change owner of folder konsultdata to group**

>sudo chmod -750 /var/systementor/konsultdata/

**Give group "konsulter" write privelige to file anteckningar.txt**

>sudo chmod -R 640 /var/systementor/konsultdata/anteckningar.txt

**Verify priveliges**

>vboxuser@Ubuntu:/var/systementor/konsultdata$ ls -al
total 8
d----w-r-x 2 root konsulter 4096 Oct  1 21:35 .
drwxr-xr-x 3 root root      4096 Oct  1 21:34 ..
-rw-r----- 1 root konsulter    0 Oct  1 21:35 anteckningar.txt
vboxuser@Ubuntu:/var/systementor/konsultdata$

**Verify network connection to win11 box**

>vboxuser@Ubuntu:/var/systementor/konsultdata$ ping 192.168.1.50

PING 192.168.1.50 (192.168.1.50) 56(84) bytes of data.
^C      
--- 192.168.1.50 ping statistics ---
388 packets transmitted, 0 received, 100% packet loss

Win11 firewall blocks ICMP as standard.
Run `Set-NetFirewallRule -Name CoreNet-Diag-ICMP4-EchoRequest-In -enabled True` in Powershell as admin in windows11 machine to allow inbound ICMP.

**Try ping again**
>vboxuser@Ubuntu:/var/systementor/konsultdata$ ping 192.168.1.50

PING 192.168.1.50 (192.168.1.50) 56(84) bytes of data.
64 bytes from 192.168.1.50: icmp_seq=1 ttl=128 time=3.31 ms
64 bytes from 192.168.1.50: icmp_seq=2 ttl=128 time=2.08 ms
64 bytes from 192.168.1.50: icmp_seq=3 ttl=128 time=2.00 ms
64 bytes from 192.168.1.50: icmp_seq=4 ttl=128 time=4.09 ms
64 bytes from 192.168.1.50: icmp_seq=5 ttl=128 time=2.18 ms
^C
--- 192.168.1.50 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4014ms
rtt min/avg/max/mdev = 2.000/2.729/4.085/0.829 ms

**Verify network adapter settings**

> vboxuser@Ubuntu:/var/systementor/konsultdata$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:2a:3c:43 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.51/24 brd 192.168.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::d639:6b45:b6dd:4259/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
vboxuser@Ubuntu:/var/systementor/konsultdata$

