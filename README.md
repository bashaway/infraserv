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
|radius|freeRADIUS|172.20.0.4|1812,1813/udp|
|dns|unbound|172.20.0.5|53/tcp,udp|
|dhcp|ISC-Kea|-|67,68/udp|
|tftp|tftp-server|-|69/udp|



```
# ip add
111: docker1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:92:ac:38:61 brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.1/24 brd 172.20.0.255 scope global docker1
       valid_lft forever preferred_lft forever
    inet6 fe80::42:92ff:feac:3861/64 scope link
       valid_lft forever preferred_lft forever
```


```
#   docker network inspect infraserv_infranet
[
    {
        "Name": "infraserv_infranet",
        "Id": "277077f22711a65e83fff3926de4c9c9e094a40b7cd101cbf30847f67585d300",
        "Created": "2020-02-19T07:35:36.255024768+09:00",
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
        "Containers": {
            "0327c4b8b84efa398ef3d2c359198d1e5d0220d54abfe618ed0a3d8d735fd253": {
                "Name": "proxy",
                "EndpointID": "ffd737136f8b23123e9f5a77475472b9213efa5f2f61232d4f4aa30dc0601304",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/24",
                "IPv6Address": ""
            },
            "6bf6bbf299129aeed1780902768cb84827bf7b555bf2502961240299baac5f8f": {
                "Name": "radius",
                "EndpointID": "5a2cf07a1267826e885adcee8d4263fb46925d449bc1ee086e2067c70ec9a755",
                "MacAddress": "02:42:ac:14:00:04",
                "IPv4Address": "172.20.0.4/24",
                "IPv6Address": ""
            },
            "753116cce7d8556d8c9604caa90f2b34877da031f8217590d994b8c2a888234e": {
                "Name": "dns",
                "EndpointID": "d46b7d0013c9964458ef7cf3b671329a6197423dd437fc321158485b6a4e3bb6",
                "MacAddress": "02:42:ac:14:00:05",
                "IPv4Address": "172.20.0.5/24",
                "IPv6Address": ""
            },
            "f983fdd7b569b9e6838c2e907beb0081b50027511da65abce1e4b6e8b36eb2f9": {
                "Name": "syslog",
                "EndpointID": "49c0c6d9aedf1ef944b73f7e370d55670c502ca860c73bb21100247aace15cc8",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/24",
                "IPv6Address": ""
            }
        },
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
docker ps -aq | xargs docker rm -f ; \
docker images -aq | xargs docker rmi ;\
docker volume ls -qf dangling=true | xargs -r docker volume rm
```

# ref
https://docs.docker.com/compose/install/
