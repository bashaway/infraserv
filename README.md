# network service servers

Network Parameters

|key|value|
|:-:|:-:|
|name|infraserv_infranet|
|subnet|172.20.0.0/24|
|interface|docker1|

Container Parameters

|server|app|address|listen|
|:-:|:-:|:-:|:-:|
|proxy|squid|172.20.0.2|8080/tcp|
|syslog|rsyslog|172.20.0.3|514/udp|
|radius|freeRADIUS|172.20.0.4|1812/udp|
|dhcp|ISC-Kea|172.20.0.5|67/udp|
|tftp|tftp-server|-|69/udp|



```
# ip add
218: docker1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:6f:11:6c:42 brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.1/24 brd 172.20.0.255 scope global docker1
       valid_lft forever preferred_lft forever
    inet6 fe80::42:6fff:fe11:6c42/64 scope link
       valid_lft forever preferred_lft forever
```


```
# docker network inspect infraserv_infranet
[
    {
        "Name": "infraserv_infranet",
        "Id": "d49c272e4fd58fb17fe0b5a6ef1925f78f51d5a7a4536f846ea7487051d0a30d",
        "Created": "2020-02-15T16:12:48.576435339+09:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.20.0.0/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker1"
        },
        "Labels": {
            "com.docker.compose.network": "infranet",
            "com.docker.compose.project": "infraserv",
            "com.docker.compose.version": "1.25.3"
        }
    }
]
```


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
curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
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

## STOP and REMOVE All Containers
```
docker-compose stop
docker-compose rm -f
```

## Stop and remove containers, networks, images, and volumes
```
docker-compose down
```

## docker command : Clean Up All Container and Images
```
docker ps -aq | xargs docker rm -f 
docker images -aq | xargs docker rmi
```

# ref
https://docs.docker.com/compose/install/
