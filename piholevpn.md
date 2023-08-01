# setting up pihole, pivpn & ufw

## install

follow the automated install steps & configure pihole with pivpn (restict to local!)

```
curl -sSL https://install.pi-hole.net | bash
```
```
curl -L https://install.pivpn.io | bash
```
```
apt install ufw
```

## set up firewall

```
ufw allow 80/tcp
```
```
ufw allow 53/tcp
```
```
ufw allow 53/udp
```
```
ufw allow ssh_port/tcp
```
```
ufw allow 51820/udp
```

or take a look at your other open ports
```
netstat -tulpen
```

route vpn to lokal & back

```
ufw route allow in on wg0 out on eth0
```

```
ufw route allow in on eth0 out on wg0
```

activate the firewall

```
ufw enable
```

## restrict pihole webinterface to vpn

get the vpn network adress
```
ip a
```

and copy the ip of wg0 network interface (10.139.106.1)


```
nano /etc/lighttpd/lighttpd.conf
```

add copy

```
server.modules += ( "mod_access" )

$HTTP["remoteip"] != "10.139.106.1/24" {
  url.access-deny = ("")
}
```

restart the webserver

```
systemctl restart lighttpd
```


