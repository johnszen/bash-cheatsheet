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

### working with bind

```
sudo tail -f /var/log/messages
```

```
sudo systemctl status named

sudo rndc status

sudo systemctl restart named

# check named.conf syntax and etc
sudo named-checkconf
```

Copy sample from doc
```
cp /usr/share/doc/bind-9.11.4/sample/etc/named.conf /etc/named.conf
```

test
```
nslookup www.google.com localhost
nslookup test.domain localhost
nslookup localhost localhost
```

change local dns at /etc/resolv.conf
test
```
nslookup www.google.com
nslookup test.domain
nslookup localhost
nslookup 172.16.1.2
```
### sample zone file

```
$TTL 3600
@ IN SOA ns1.test.domain. root (1 3H 15M 1W 1D)

		IN	NS	ns1
		IN	MX	10	mx1.mydomain.com
		IN	MX	50	mx2.mydomain.com

ns1		IN	A	10.0.0.4
www	60	IN	A	172.16.1.2
portal		IN	CNAME	www.test.domain.
```
