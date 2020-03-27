# SSH PUBLIC KEY AUTHENTICATION

## 1. CONNECT TO SERVER AND CREATE .ssh/authorized_keys ON USERFOLDER
```
[nadia@localhost ~]$ mkdir .ssh
[nadia@localhost ~]$ chmod 700 .ssh/
[nadia@localhost ~]$ touch .ssh/authorized_keys
[nadia@localhost ~]$ chmod 600 .ssh/authorized_keys
```
## 2. GENERATE PUBLIC AND PRIVATE KEY USING PUTTYGEN AND COPY PUBLIC KEY INTO .ssh/authorized_keys, SAVE PRIVATE KEY ON CLIENT'S COMPUTER

```
sudo nano .ssh/authorized_keys
```

## 3. EDIT /etc/ssh/sshd_config
Change PasswordAuthentication yes to PasswordAuthentication no

Change usepam yes to usepam no

## 4. RESTART sshd



