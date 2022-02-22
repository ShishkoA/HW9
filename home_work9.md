## 1. add secondary ip address to you second network interface enp0s8. Each point must be presented with commands and showing that new address was applied to the interface. To repeat adding address for points 2 and 3 address must be deleted (please add deleting address to you homework log) Methods:
   1. using ip utility (stateless)
```bash 
 [root@localhost linux]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defaul                 t qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP g                 roup default qlen 1000
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe61:815b/64 scope link
       valid_lft forever preferred_lft forever
[root@localhost linux]# ip address add 192.168.0.135/24 dev ens33
[root@localhost linux]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defaul                 t qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP g                 roup default qlen 1000
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.0.135/24 scope global secondary ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe61:815b/64 scope link
       valid_lft forever preferred_lft forever
[root@localhost linux]#
```
2. using centos network configuration file (statefull)
  ```bash 
   [root@localhost linux]# cat /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=f999d087-984b-4563-93c4-549f65a81283
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.0.133
PREFIX=24
GATEWAY=192.168.0.1
DNS1=8.8.8.8
IPADDR1=192.168.0.135
PREFIX1=24
```
   3. using nmcli utility (statefull)
```bash
   [root@localhost linux]# nmcli con mod ens33 +ipv4.addresses "192.168.0.135/24"
[root@localhost linux]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft foreverC
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe61:815b/64 scope link
       valid_lft forever preferred_lft forever
[root@localhost linux]# reboot
Using username "linux".
Authenticating with public key "rsa-key-20211006" from agent
Last login: Fri Dec 24 00:16:55 2021 from 192.168.0.31
[linux@localhost ~]$ sudo su
[sudo] password for linux:
[root@localhost linux]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.0.135/24 brd 192.168.0.255 scope global secondary noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe61:815b/64 scope link
       valid_lft forever preferred_lft forever
```
## 2. You should have a possibility to use ssh client to connect to your node using new address from previous step. Run tcpdump in separate tmux session or separate connection before starting ssh client and capture packets that are related to this ssh connection. Find packets that are related to TCP session establish.
```bash
[root@localhost linux]# ip a                                                                                          │02:07:21.803326 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10581776, win 256, length 0
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000                           │02:07:21.803542 IP 192.168.0.135.22 > 192.168.0.31.14572: Flags [P.], seq 10581776:10583392, ack 8321, win 286, length
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                             │ 1616
    inet 127.0.0.1/8 scope host lo                                                                                    │02:07:21.803798 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10583392, win 256, length 0
       valid_lft forever preferred_lft forever                                                                        │02:07:21.804012 IP 192.168.0.135.22 > 192.168.0.31.14572: Flags [P.], seq 10583392:10587552, ack 8321, win 286, length
    inet6 ::1/128 scope host                                                                                          │ 4160
       valid_lft forever preferred_lft forever                                                                        │02:07:21.804337 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10586312, win 256, length 0
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000                │02:07:21.804479 IP 192.168.0.135.22 > 192.168.0.31.14572: Flags [P.], seq 10587552:10589136, ack 8321, win 286, length
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff                                                                │ 1584
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33                                          │02:07:21.804748 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10589012, win 256, length 0
       valid_lft forever preferred_lft forever                                                                        │02:07:21.804924 IP 192.168.0.135.22 > 192.168.0.31.14572: Flags [P.], seq 10589136:10593296, ack 8321, win 286, length
    inet 192.168.0.135/24 brd 192.168.0.255 scope global secondary noprefixroute ens33                                │ 4160
       valid_lft forever preferred_lft forever                                                                        │02:07:21.805246 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10590596, win 256, length 0
    inet6 fe80::20c:29ff:fe61:815b/64 scope link                                                                      │02:07:21.805327 IP 192.168.0.31.14572 > 192.168.0.135.22: Flags [.], ack 10593296, win 256, length 0
       valid_lft forever preferred_lft forever 
	   
	   tcpdump -i ens33 'ip proto \tcp'
```
## 3. Close session. Find in tcpdump output packets that are related to TCP session closure.
```bash
[root@localhost linux]# ip a                                                                                          │$[root@localhost linux]# tcpdump -i ens33 "tcp[tcpflags] & (tcp-fin) != 0"
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000                           │tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00                                                             │listening on ens33, link-type EN10MB (Ethernet), capture size 262144 bytes
    inet 127.0.0.1/8 scope host lo                                                                                    │16:22:26.254431 IP localhost.localdomain.60236 > ya.ru.http: Flags [F.], seq 2803486643, ack 3070002651, win 239, opti
       valid_lft forever preferred_lft forever                                                                        │ons [nop,nop,TS val 4149048 ecr 3875346700], length 0
    inet6 ::1/128 scope host                                                                                          │16:22:26.272382 IP ya.ru.http > localhost.localdomain.60236: Flags [F.], seq 1, ack 1, win 170, options [nop,nop,TS va
       valid_lft forever preferred_lft forever                                                                        │l 3875346718 ecr 4149048], length 0
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000                │
    link/ether 00:0c:29:61:81:5b brd ff:ff:ff:ff:ff:ff                                                                │
    inet 192.168.0.133/24 brd 192.168.0.255 scope global noprefixroute ens33                                          │
       valid_lft forever preferred_lft forever                                                                        │
    inet 192.168.0.135/24 brd 192.168.0.255 scope global secondary noprefixroute ens33                                │
       valid_lft forever preferred_lft forever                                                                        │
    inet6 fe80::20c:29ff:fe61:815b/64 scope link                                                                      │
       valid_lft forever preferred_lft forever                                                                        │
[root@localhost linux]# curl ya.ru                                                                                    │
```
## 4. run tcpdump and request any http site in separate session. Find HTTP request and answer packets with ASCII data in it.  Tcpdump command must be as strict as possible to capture only needed packages for this http request.
```bash
tcpdump -A -i ens33 'tcp port http'
```
