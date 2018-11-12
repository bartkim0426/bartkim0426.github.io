---
layout: post
title:  "linux: Ubuntu Firewall Open Port Command"
categories: linux
---


## See the firewall


```bash
sudo ufw status verbose
```

## Open port

```bash
# allow 22 port
sudo ufw allow 22/tcp

# allow 80/443
sudo ufw allow http
sudo ufw allow https
# or this works too
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp


# advanced usage
# To allow IP address 192.168.1.10 access to port 22 for all protocols
sudo ufw allow from 192.168.1.10 to any port 22

# Open port 74.86.26.69:443 (SSL 443 nginx/apache/lighttpd server) for all, enter:
sudo ufw allow from any to 74.86.26.69 port 443 proto tcp

# To allows subnet 192.168.1.0/24 to Sabma services, enter:
ufw allow from 192.168.1.0/24 to any app Samba
```
