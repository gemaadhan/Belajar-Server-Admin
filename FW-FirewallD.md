# FIREWALLD

FirewallD adalah frontend controller untuk iptables. FirewallD menggunakan zone dan service berbeda dengan iptables yang menggunakan chain dan rules. 

## SETUP FIREWALLD

### START SERVICE
pada centos 7 firewallD sudah include by default, tapi belum aktif.
```
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

### STOP SERVICE
```
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

### CHECK STATUS
```
sudo firewall-cmd --state
sudo systemctl status firewalld
```

### RESTART SERVICE
```
sudo firewall-cmd --reload
```
## CONFIG

### FIREWALL ZONE
zone atau saya sebut sebagai profil merupakan kumpulan rule yang diapply pada interface. sudah ada default zone bawaan firewalld. interface yang ga diset zone nya akan menggunakan default zone. 

```
sudo firewall-cmd --get-default-zone //melihat defaultzone
sudo firewall-cmd --set-default-zone=internal //menset defaultzone
sudo firewall-cmd --get-active-zones //melihat active zone
sudo firewall-cmd --zone=public --list-all //melihat semua configurasi dari suatu zone
sudo firewall-cmd --list-all-zones //melihat semua konfigurasi zones
```

### FIREWALL SERVICE
```
sudo firewall-cmd --get-services //melihat list service yang tersedia
sudo firewall-cmd --zone=public --add-service=http --permanent //allow service
sudo firewall-cmd --zone=public --remove-service=http --permanent //hapus service
```

### FIREWALL PORT
selain service bisa juga menggunakan port
```
sudo firewall-cmd --zone=dmz --add-port=12345/tcp --permanent //allow port
sudo firewall-cmd --zone=dmz --remove-port=12345/tcp --permanent //hapus port
sudo firewall-cmd --zone="dmz" --add-forward-port=port=80:proto=tcp:toport=22 //port forwarding internal
sudo firewall-cmd --zone="dmz" --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=198.51.100.1 //port forwarding keserver lain
sudo firewall-cmd --zone=dmz --remove-masquerade //jangan lupa
```

### FIREWALL ZONE AGAIN
```
sudo firewall-cmd --zone=dmz --add-interface=eth0 //tambah interface di zone
sudo firewall-cmd --zone=dmz --change-interface=eth0 //merubah interface pada zone. 
sudo firewall-cmd --permanent --new-zone=namazonebaru //membuat zone kita sendiri, jangan lupa reload.
```

### RICH RULES
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address=192.168.1.2 accept'//accept trafik dari source tertenu
sudo firewall-cmd --add-rich-rule 'rule family="ipv4" source address=192.168.1.7 port=22 protocol=tcp reject' //reject port 22 dari ip tertentu
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 source address=192.0.2.0 forward-port port=80 protocol=tcp to-port=6532' //forward req ke port 80 ke 6532 di internal
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 forward-port port=80 protocol=tcp to-port=8080 to-addr=198.51.100.0' //forward req port 80 ke 8080 ke komputer lain
sudo firewall-cmd --zone=public --list-rich-rules //checkrule
```









