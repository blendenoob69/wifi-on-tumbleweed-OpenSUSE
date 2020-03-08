# wifi-on-tumbleweed-OpenSUSE
openSUSE Tumbleweed trouble with WIFI dongle

Past history:
I just wanted to test out the VPN on a linux distribution. There for I took an old DELL-Laptop and installed openSUSE Tumbleweed(386) on it. OpenSUSE, because I started my LINUX experience with SUSE. With other distros, I’m not so conform.
It started well. I installed whole KDE GUI and what not. I have been able to connect to the internet. All went well, till I decided to configure the local LAN interface to activate squid. I wanted to hold the out reach of the computer as low as possible. It was not possible to configure the network so that I could reach through the LAN to the net with a WIN8 pc. So I decided to switch to WICKED. So now I was able to access the DELL-Laptop but now I was unable to activate the WIFI connection on the DELL-Laptop. Switched back to NETWORK-MANAGER. Now I was able to activate WIFI but I was not able to configure the LAN so that I could access it with WIN8 pc. Switched back to WICKED, now the FIREWALL crashed. After investigating several hours on the topic, I decided to reinstall OpenSUSE. After several tries and several reinstallations I downloaded DEBIAN. On DEBIAN I was not able to activate the WIFI. Then the decision to stay on OpenSUSE. After 4 days I was able to configure WIFI, LAN to function reliable. 

Hardware
[WIN8-pc] -----LAN-----[SWITCH]-----[DELL-Laptop]-----WIFI-----[INTERNET]


DELL-Laptop#> inxi
CPU: Dual Core Intel Core2 T5200 (-MCP-) speed/min/max: 798/800/1600 MHz
Kernel: 5.5.6-1-pae i686 Up: 21h 21m 
Mem: 818.8/1941.9 MiB (42.2%)
Storage: 305.55 GiB (3.1% used) 
Procs: 192 Shell: bash 5.0.16 
inxi: 3.0.32

DELL-Laptop#>uname -a
Linux dellBOX 5.5.6-1-pae #1 SMP Mon Feb 24 09:02:31 UTC 2020 (4a830b1) i686 i686 i386 GNU/Linux

DELL-Laptop#>yast
 YaST2 - lan @ pcname

Network Settings
l Global Options	-	*Overview	-	Hostname/DNS	-	Routing
l--------------------------------------------------------------------------------------------------------------------|
| Name                                           					IP Address    		Device  |
| BCM4401-B0 100Base-TX                          				172.16.1.1/16 	eth0     |
| PRO/Wireless 3945ABG [Golan] Network Connectionx	192.168.1.2/24	wlp11s0  |
| enp0s29f7u3                                    					DHCP+AUTOIP	enp0s29f7u3|
| wlp0s29f7u3                                   					DHCP4         		wlp0s29f7u3|
|---------------------------------------------------------------------------------------------------------------------------
|---------------------------------------------------------------------------------------------------------------------------
| BCM4401-B0 100Base-TX
| MAC : xx:XX:yy:YY:xx:XX                                                 |
| BusID : 0000:03:00.0                                                    vx
| *  Device Name: eth0                                                   |
| *  Configured with address 172.16.1.1/16(pcname)                      |
|-----------------------------------------------------------------------------------------------------------------------
| [Add][Edit][Delete]
|
|-----------------------------------------------------------------------------------------------------------------------
 [Help]                                        [Cancel]                 [ OK ]


Network Settings
l *Global Options	-	Overview	-	Hostname/DNS	-	Routing
l--------------------------------------------------------------------------------------------------------------------|
| Network Setup Method
| Wicked Service
| IPv6 Protocol Settings
| [x] Enable IPv6
l--------------------------------------------------------------------------------------------------------------------|
| IPv6 Protocol Settings
| [x] Enable IPv6
l--------------------------------------------------------------------------------------------------------------------|
| Hostname to Send
| AUTO
| [x] Change Default Route via DHCP 
l--------------------------------------------------------------------------------------------------------------------|
[Help]                                              [Cancel]                    [ OK ]


Network Settings
l Global Options	-	Overview	-	*Hostname/DNS	-	Routing
l--------------------------------------------------------------------------------------------------------------------|
| Hostname
| DELL-Laptop
| Set Hostname via DHCP  no↓
|
| Modify DNS Configuration Custom Policy Rule
| Use Custom Policy↓ 	STATIC↓  
| Name Server 1 
| 8.8.8.8
| Name Server 2
| 4.4.4.4
| Name Server 3
| 9.9.9.9
l--------------------------------------------------------------------------------------------------------------------|
[Help]                                              [Cancel]                    [ OK ]



Network Settings
l Global Options	-	Overview	-	Hostname/DNS	-	*Routing
l--------------------------------------------------------------------------------------------------------------------|
| [x] Enable IPv4 Forwarding
| [x] Enable IPv6 Forwarding
l--------------------------------------------------------------------------------------------------------------------|
| Routing Table
l--------------------------------------------------------------------------------------------------------------------|
| DestinationxGateway       Device     Options
| default     192.168.42.236	enp0s29f7u3
l--------------------------------------------------------------------------------------------------------------------|
 [Add][Edit][Delete]  
l--------------------------------------------------------------------------------------------------------------------|
[Help]                                              [Cancel]                    [ OK ]


DELL-Laptop#>ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff
    inet 172.16.1.1/16 brd 172.16.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::219:b9ff:fe5d:1e61/64 scope link
       valid_lft forever preferred_lft forever
3: wlp11s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.2/24 brd 192.168.1.255 scope global wlp11s0
       valid_lft forever preferred_lft forever
    inet6 fe80::219:d2ff:fe7c:8a49/64 scope link
       valid_lft forever preferred_lft forever

DELL-Laptop#>route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.1     0.0.0.0         UG    0      0        0 wlp11s0
172.16.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth0
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 wlp11s0

Solution
ip a
ip link set wlp11s0 up / down
wpa_supplicant -B -i wlp11s0 -c /etc/wpa_supplicant/wpa_supplicant.conf 
ifconfig wlp11s0 192.168.1.2/24
dhclient -4 wlp11s0
ip route add default via 192.168.1.1 

Epilogue 
I’m not a network , system nor linux expert. I share this because I succeeded the topic and thought it would be helpful to share the solution. I missed such a gethered  solution.
To run this on boot I will write a bash-file.

Index
whereis
ip: /usr/sbin/ip
wpa_supplicant: /usr/sbin/wpa_supplicant
ifconfig: /usr/bin/ifconfig
dhclient: /sbin/dhclient
yast: /usr/sbin/yast
route: /usr/bin/route 

etc/wpa_supplicant/wpa_supplicant.conf 
/etc/sysconfig/network/ifcfg-wlp11s0 
