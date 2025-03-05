# Hosting setup

## Generating SSH key

First generate a SSH key on the device on which you intend to connect to a remote SSH server

```bash
ssh-keygen -t ed25519 -C "votre commentaire"
```

## Setting up remote SSH server

In this tuto we are assuming that your server is running on Debian

### Creating a new user

***Note :*** It is important to create a user and not rely on root user, because if someone manages to get access to root user, it's game over.

```sh
apt update
apt install sudo
# add new user
adduser <username>

# OR 

useradd -m -s /bin/bash <username> # -m to create a home folder and -s to specify the default shell
# create password for new user
passwd <username>
# add user to sudo group
usermod -aG sudo <username>
# connect with the new user
su <username>
# (optional) change default shell
sudo usermod --shell /bin/bash <username>
```
### Create SSH access
```sh
mkdir ~/.ssh
nano .ssh/authorized_keys
# paste content of public ssh key in the authorized_keys file

sudo nano /etc/ssh/sshd_config

#Change default port (22)
#Disabled Root login and disable password authentification

#/etc/ssh/sshd_config
# ...
# Port XX
# ...
# PermitRootLogin no
# ...
# PasswordAuthentication no

# restart ssh service
sudo systemctl restart sshd
# OR
sudo reload sshd

# reconnect
ssh <username>@<remote_server_ip> -p <port>
```
### Disabling Root user

To disable, you can **remove the password** of the account or **lock it down**, or even do both of them:

Remove the root password:
```bash
sudo passwd -d root
```
Lock the account:
```bash
sudo passwd -l root
```


### Setting up Docker

Install docker engin for Debian : [doc](https://docs.docker.com/engine/install/debian/)

***Note :*** It is a bad practice to user docker with `sudo`, to avoid this we need to add user to `docker` group :
```bash
# add user to docker group
sudo usermod -aG docker <username>
# OR 
usermod -aG docker $USER

# reconnect
exit
ssh <username>@<remote_server_ip> -p <port>

mkdir endlessh
cd endlessh
# get endlessh script
nano docker-compose.yml
docker compose up  -d
```