---
title: Pihole DNS forwarder to Multiple Domain
date:  2024-12-16
categories: [Networking,Docker]
tags: [pihole,dns,domain,config,docker]
---
![image info](/assets/images/pihole-dns-forwarder.png)

# Config for Multiple Domain and Multiple VLAN

### Environment:
Multiple DNS Domain, Multiple VLAN, Multiple Remote DNS Domain Via VPN Tunnel
### Challenge:
Configure multiple DNS forwarder in Pihole
### Problem:
Pihole DNS forwarder only allow setup 1 entry from Gui
### Solution:
Create Customized config file
### Steps:

1. SSH into Pihole VM/Container,  go to /etc/dnsmasq.d
2. created a customized config file , example: 02-custom.conf
3. add content blow
```config
# Local domain 1 on AD DNS1
server=/mydomain1.local/192.168.128.66
# Local domain 1 on AD DNS2
server=/mydomain1.local/192.168.128.220
# Local domain 2  on AD DNS1
server=/mydomain2.com/192.168.128.66
# Local domain 2 on AD DNS2
server=/mydomain2.com/192.168.128.220
# Local DNS1 will forward request again to Remote AD DNS1 via VPN1
server=/remote-domain1.local/192.168.128.66
# Local DNS2 will forward request again to Remote AD DNS1 via VPN1
server=/remote-domain1.local/192.168.128.220
#Local DNS1 will forward request again to Remote AD DNS1 via VPN2
server=/remote-domain2.biz/192.168.128.66
#Local DNS2 will forward request again to Remote AD DNS1 via VPN2
server=/remote-domain2.biz/192.168.128.220 
# reverse lookup method 1
server=/128.168.192.in-addr.arpa/192.168.128.66
server=/128.168.192.in-addr.arpa/192.168.128.220
server=/20.16.172.in-addr.arpa/192.168.128.66
server=/20.16.172.in-addr.arpa/192.168.128.220
# reverse lookup method 2
rev-server=172.16.30.0/24,192.168.128.66
rev-server=172.16.30.0/24,192.168.128.220
rev-server=172.16.40.0/24,192.168.128.66
rev-server=172.16.40.0/24,192.168.128.220
rev-server=172.16.50.0/24,192.168.128.66
rev-server=172.16.50.0/24,192.168.128.220
rev-server=172.16.70.0/24,192.168.128.66
rev-server=172.16.70.0/24,192.168.128.220
rev-server=172.16.80.0/24,192.168.128.66
rev-server=172.16.80.0/24,192.168.128.220
rev-server=10.107.0.0/16,10.107.10.30
rev-server=10.107.0.0/16,10.107.10.31
rev-server=10.100.0.0/16,10.100.10.50
rev-server=10.100.0.0/16,10.100.10.51
```
1.  Consult dnsmasq usage, parameters, format , <https://thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html>



#### DNS Request to remote Domain
1. Pihole will forward DNS request to local AD ,and AD DNS will forward DNS request to remote DNS through VPN Tunnel.
2. Reversed DNS request to remote DNS domain, will forward directly to remote DNS server by Pihole.