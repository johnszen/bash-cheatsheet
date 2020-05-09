# bind

## prepare

### test ssh key passphrase.

```
ssh-keygen -y -f ~/.ssh/id_rsa
```

### ssh

```
ssh username@machine
```

### os 

```
cat /etc/os-release
# NAME="CentOS Linux"
# VERSION="7 (Core)"
# ID="centos"
```

## install

### is bind installed

```
sudo service --status-all
sudo service named status
systemctl
# check /etc/bind
```

### install

```
# is yum available
which yum
yum version

# install
sudo yum install bind bind-utils -y
# check if /etc/named.conf and /etc/named

# enable it to start on boot
sudo systemctl enable named
#Created symlink from /etc/systemd/system/multi-user.target.wants/named.service to /usr/lib/systemd/system/named.service.

# start it
sudo systemctl start named

# check
systemctl | grep named
# voila! it is there
# named.service
```
