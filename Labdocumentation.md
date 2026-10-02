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

```bash
vboxuser@Ubuntu:~$ sudo mkdir /var/systementor
```

```bash
vboxuser@Ubuntu:~$ sudo mkdir /var/systementor/konsultdata
```

**Create file**

```bash
vboxuser@Ubuntu:~$ sudo touch /var/systementor/konsultdata/anteckningar.txt
```

**Create group "konsulter"**

```bash
vboxuser@Ubuntu:~$ sudo addgroup konsulter
```

**Change owner of folder konsultdata to group**

```bash
vboxuser@Ubuntu:~$ sudo chmod -750 /var/systementor/konsultdata/
```

**Give group "konsulter" write privelige to file anteckningar.txt**

```bash
vboxuser@Ubuntu:~$ sudo chmod -R 640 /var/systementor/konsultdata/anteckningar.txt
```

**Verify priveliges**

```bash
vboxuser@Ubuntu:/var/systementor/konsultdata$ ls -al
total 8
d----w-r-x 2 root konsulter 4096 Oct  1 21:35 .
drwxr-xr-x 3 root root      4096 Oct  1 21:34 ..
-rw-r----- 1 root konsulter    0 Oct  1 21:35 anteckningar.txt
vboxuser@Ubuntu:/var/systementor/konsultdata$
```

**Verify network connection to win11 box**

```bash
vboxuser@Ubuntu:/var/systementor/konsultdata$ ping 192.168.1.50
PING 192.168.1.50 (192.168.1.50) 56(84) bytes of data.
^C      
--- 192.168.1.50 ping statistics ---
388 packets transmitted, 0 received, 100% packet loss
```

Win11 firewall blocks ICMP as standard.
Run `Set-NetFirewallRule -Name CoreNet-Diag-ICMP4-EchoRequest-In -enabled True` in Powershell as admin in windows11 machine to allow inbound ICMP.

**Try ping again**
```bash
vboxuser@Ubuntu:/var/systementor/konsultdata$ ping 192.168.1.50
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
```

**Verify network adapter settings**

```bash
vboxuser@Ubuntu:/var/systementor/konsultdata$ ip addr show
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
```

**Windows**

Open powershell as admin

**Create directory c:\sytementor\konsultdata**

```
PS C:\WINDOWS\system32> mkdir c:\Systementor\konsultdata


    Directory: C:\Systementor


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         10/2/2026   1:56 AM                konsultdata


PS C:\WINDOWS\system32>
```

**Get ACL**

```PS C:\WINDOWS\system32> get-acl c:\Systementor\konsultdata


    Directory: C:\Systementor


Path        Owner                  Access
----        -----                  ------
konsultdata BUILTIN\Administrators BUILTIN\Administrators Allow  FullControl...


PS C:\WINDOWS\system32>
```

**Verify network connectivity to Ubuntu (192.168.1.51)**

```
PS C:\WINDOWS\system32> ping 192.168.1.51

Pinging 192.168.1.51 with 32 bytes of data:
Reply from 192.168.1.51: bytes=32 time=2ms TTL=64
Reply from 192.168.1.51: bytes=32 time=1ms TTL=64
Reply from 192.168.1.51: bytes=32 time=2ms TTL=64
Reply from 192.168.1.51: bytes=32 time=1ms TTL=64

Ping statistics for 192.168.1.51:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 2ms, Average = 1ms
PS C:\WINDOWS\system32>
```

**Display Network configuration**

```
PS C:\WINDOWS\system32> ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : Win11
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Intel(R) PRO/1000 MT Desktop Adapter
   Physical Address. . . . . . . . . : 08-00-27-C1-14-36
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::dc8a:ba1d:7871:99fb%5(Preferred)
   IPv4 Address. . . . . . . . . . . : 192.168.1.50(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.1.1
   DHCPv6 IAID . . . . . . . . . . . : 84410407
   DHCPv6 Client DUID. . . . . . . . : 00-01-01-00-32-2A-13-74-08-00-27-C1-14-36
   DNS Servers . . . . . . . . . . . : 8.8.8.8
   NetBIOS over Tcpip. . . . . . . . : Enabled
PS C:\WINDOWS\system32>
```