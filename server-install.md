# Server Installation
Step by Step Guide for how to install a Ubuntu 20.4 Server with Traefik and Docker Compose.

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

## III. Docker
### Install docker-ce
```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Install Docker Engine
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
Add user to docker group.
```
sudo usermod -aG docker $USER
```

### Install docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### Trouble Shooting
After install 23.0.1 on the Ubuntu 20.4 docker is broken.
Fixed it like that: ```apt install apparmor``` and rebooted host.

Quellen:
* https://community.hetzner.com/tutorials/howto-initial-setup-ubuntu
* https://docs.docker.com/engine/install/ubuntu/
* https://www.digitalocean.com/community/tutorials/how-to-use-traefik-v2-as-a-reverse-proxy-for-docker-containers-on-ubuntu-20-04
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04
* https://doc.traefik.io/traefik/getting-started/quick-start/



