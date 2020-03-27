# DNS CONFIGURATION CENTOS 7

## IP DNS SERVER = 192.168.1.15
## DOMAIN : gemaadhan.local

```bash
sudo nano /etc/named.conf
```
```
listen-on port 53 { 127.0.0.1; 192.168.1.15;}; ### Master DNS IP ###
...
allow-query     { localhost; any;}; ### IP Range ###
...
zone "gemaadhan.local" IN {
        type master;
        file "forward.gemaadhan";
        allow-update { none; };
};
zone "168.192.in-addr.arpa" IN {
        type master;
        file "reverse.gemaadhan";
        allow-update { none; };
};
```

```
sudo nano /var/named/forward.gemaadhan
```
```
$TTL 86400
@   IN  SOA     masterdns.gemaadhan.local. root.gemaadhan.local. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.gemaadhan.local.
@       IN  A           192.168.1.15
masterdns       IN  A   192.168.1.15
```
```
@   IN  SOA     masterdns.gemaadhan.local. root.gemaadhan.local. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.gemaadhan.local.
@       IN  PTR         gemaadhan.local.
masterdns       IN  A   192.168.1.15
15     IN  PTR         masterdns.gemaadhan.local.
```

## CHECK ALL FILES
```
named-checkconf /etc/named.conf
named-checkzone gemaadhan.local /var/named/forward.gemaadhan
named-checkzone gemaadhan.local /var/named/reverse.gemaadhan
```

## START DNS SERVICE
systemctl enable named
systemctl start named

## ENABLE FW 
firewall-cmd --permanent --add-port=53/tcp
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --reload


