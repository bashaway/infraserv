# network service servers

## proxy

squid proxy server

listen: 8080/tcp


## syslog

rsyslog

listen: 514/udp


## radius

freeradius

listen: 1812/udp


## dhcp

kea-dhcp

listen : 67/udp


## tftp

tftp-server + xinetd

listen : 69/udp


## docker network

name: infraserv_infranet
subnet: 172.20.0.0/24
host if name: docker1



# Install Docker

## DockerCE

```
dnf -y update
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf -y --nobest install docker-ce docker-ce-cli containerd.io
dnf -y update https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.10-3.2.el7.x86_64.rpm
dnf -y update
systemctl enable docker
systemctl start docker
```

## Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

# Firewall Policy add
```
firewall-cmd --add-port=8080/tcp  --zone=public --permanent
firewall-cmd --add-service=syslog --zone=public --permanent
firewall-cmd --add-service=radius --zone=public --permanent
firewall-cmd --add-service=dhcp   --zone=public --permanent
firewall-cmd --add-service=tftp   --zone=public --permanent
firewall-cmd --add-masquerade --permanent
firewall-cmd --reload
```


# build servers

## git clone
```
git clone https://github.com/bashaway/infraserv
```

## Build and Start Containers
```
cd infraserv
docker-compose build
docker-compose up -d
```

# remove servers

# STOP and REMOVE All Containers
```
docker-compose stop
docker-compose rm -f
```

# REMOVE All IMAGES
```
docker-compose rmi -f
```

## docker command : Clean Up All Container and Images
```
docker ps -aq | xargs docker rm -f 
docker images -aq | xargs docker rmi
```

# ref
https://docs.docker.com/compose/install/
