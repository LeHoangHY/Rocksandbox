# ROCK SANDBOX
**ROCK SANDBOX** ��?c x�y d?ng d?a tr�n QEMU d�ng cho m?c ��ch ph�n t�ch �?ng.
## Ch?c n�ng:
* Th?c thi executable file tr�n qemu-system v?i ki?n tr�c t��ng ?ng. (hi?n t?i m?i �ang h? tr? MIPS).
* Thu th?p systemcall th�ng qua c�ng c? strace.
* Thu th?p h�nh vi m?ng s? d?ng TCPDump
## INSTALL:
### Chu?n b?:
* M�y ?o Ubuntu desktop 18.04:
  * Network: 2 card m?ng
    * 1 card m?ng NAT (ens33)
    * 1 card m?ng host only (ens38)
* T?i v? m? ngu?n
* T?i v? t?p qemu image
### C�i �?t qemu:
```
sudo apt-get update
sudo apt-get install qemu
```
### C�i �?t inetsim: 
```
sudo apt-get install inetsim
```
C?u h?nh inetsim cho card m?ng host only:
```
sudo gedit /etc/inetsim/inetsim.conf
uncomment v� s?a l?i (default):
service_bind_address	192.168.1.20 
dns_default_ip		192.168.1.20
```

**T?t apache2(n?u �ang ch?y):**
```
sudo service apache2 stop
```
### C�i �?t bridge network:
```
sudo apt-get install uml-utilities bridge-utils
groupadd -r tuntap
usermod -a -G tuntap ubuntu (username)
sudo service NetworkManager stop
```
**S?a interface**
```
sudo gedit /etc/network/interfaces
```
S?a l?i nh� sau: 
```
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback
atuto ens33
iface ens38 inet dhcp
auto br0
iface br0 inet static
address 192.168.1.20
netmask 255.255.255.0
bridge_ports ens38
bridge_maxwait 0
```
Restart network
```
sudo ifdown -a
sudo ifup -a
```
Ki?m tra v?i l?nh `ifconfig`, n?u xu?t hi?n interface br0 l� ok.
### C?u h?nh switch ?o cho QEMU:
```
sudo gedit /etc/qemu-ifup
```
Th�m d?ng l?nh switch = br0
```
...
switch=$(ip route ls | \
    awk '/^default / {
          for(i=0;i<NF;i++) { if ($i == "dev") { print $(i+1); next; } }
         }'
        )
switch=br0 
...
```
### C�i �?t th� vi?n cho Python3
```
pip3 install paramiko
```
## S? d?ng
1. Kh?i �?ng qemu v?i l?nh
```
./run_qemu.sh
```
2. Ph�n t�ch sample
```
python3 ./analysis.py -s [sample] -d [destFolder] -t [timeToWait]
```
3. Sau khi ph�n t�ch thu ��?c 2 t?p : *strace.txt* v� *dump.pcap* t?i th� m?c [destFolder]



