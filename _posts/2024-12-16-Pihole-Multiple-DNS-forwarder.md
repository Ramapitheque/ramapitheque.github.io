---
title: Pihole DNS forwarder to Multiple Domain
date:  2024-12-16
categories: [Networking,Docker]
tags: [pihole,dns,domain,config,docker]
---

# Config for Multiple Domain and Multiple VLAN

Pihole is popular AD block / DNS server,  It is great for self-hosting, it is light weighted, especially after I migrated it from VM to container.
it has setting for DNS forwarder to forwarder DNS request to your own DNS server from Gui
Some people have multiple  VLAN and want DNS request to be forwarded to DNS server for each VLAN.
In my case I have 2 different domains on 2 DNS servers in Active Directory With multiple VLAN, and DNS request across multiple VPN tunnels

Pihole DNS forwarder only allow  setup 1 entry from Gui, I see many people request to allow multiple entires from Gui,  but Dev team does not want provide this , because they want this reserved for more experienced users.
Below is how to archive this,  it is quite simple
1. SSH into Pihole VM/Container,  go to /etc/dnsmasq.d
2. created a customized config file , example: 02-custom.conf
3. add content blow
```config
server=/mydomain1.local/192.168.128.66 # Local domain 1 on AD DNS1
server=/mydomain1.local/192.168.128.220 # Local domain 1 on AD DNS2
server=/mydomain2.com/192.168.128.66 # Local domain 2  on AD DNS1
server=/mydomain2.com/192.168.128.220 # Local domain 2 on AD DNS2
server=/remote-domain1.local/192.168.128.66 # Local DNS1 will forward request again to Remote AD DNS1 via VPN1
server=/remote-domain1.local/192.168.128.220 # Local DNS2 will forward request again to Remote AD DNS1 via VPN1
server=/remote-domain2.biz/192.168.128.66 #Local DNS1 will forward request again to Remote AD DNS1 via VPN2
server=/remote-domain2.biz/192.168.128.220 #Local DNS2 will forward request again to Remote AD DNS1 via VPN2
server=/128.168.192.in-addr.arpa/192.168.128.66 # reverse lookup method 1
server=/128.168.192.in-addr.arpa/192.168.128.220
server=/20.16.172.in-addr.arpa/192.168.128.66
server=/20.16.172.in-addr.arpa/192.168.128.220
rev-server=172.16.30.0/24,192.168.128.66 # reverse lookup method 2
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
