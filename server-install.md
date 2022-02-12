# Server Installation
Step by Step Guide for how to install a Ubuntu 20.4 Server with Traefik an Docker

## I. Base
Connect to your server via ssh and create a new user with sudo privileges. Please replace sami with an own username.
```
ssh root@<ip-address-server>
adduser sami
usermod -aG sudo sami
```
If you already have a ssh key installed copy it to the new user home directory. Otherwise create a new one and skip the next command
```
rsync --archive --chown=sami:sami ~/.ssh /home/sami
```
Logout and test new user
```
exit
ssh sami@<ip-address-server>
```
Update Server
```
sudo apt update
sudo apt upgrade
```

## II. Security
### Install UFW (uncomplicated firewall)
```
sudo apt ufw
```
List Available Services (optional)
```
sudo ufw app list
Available applications:
OpenSSH
```
Allow ssh connection
```
sudo ufw allow OpenSSH
```
See which Rules where added.
```
sudo ufw show added
```
Enable Firewall
```
sudo ufw enable
```
Check Status
```
sudo ufw status
```
### Deactivate Root Login
```
sudo vi /etc/ssh/sshd_config
```
Look for PermitRootLogin. Uncomment line and set it to no.
```
PermitRootLogin     no
```
Look for PasswordAuthentication. Uncomment line and set it to no.
```
PasswordAuthentication no
```
Restart ssh.
```
sudo systemctl restart sshd
```




