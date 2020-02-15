= Install Docker
dnf -y update
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf -y --nobest install docker-ce docker-ce-cli containerd.io
dnf -y update https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.10-3.2.el7.x86_64.rpm
dnf -y update
systemctl enable docker
systemctl start docker

= Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

= Build and Start Containers
docker-compose build
docker-compose up -d

= Firewall Policy add
firewall-cmd --add-port=8080/tcp  --zone=public --permanent
firewall-cmd --add-service=syslog --zone=public --permanent
firewall-cmd --add-service=radius --zone=public --permanent
firewall-cmd --add-service=dhcp   --zone=public --permanent
firewall-cmd --add-service=tftp   --zone=public --permanent
firewall-cmd --add-masquerade --permanent
firewall-cmd --reload

= Build Images
docker-compose build

= START All Containers
docker-compose up -d
docker-compose ps

= STOP and REMOVE All Containers
docker-compose stop
docker-compose rm -f

= REMOVE All IMAGES
docker-compose rmi -f

= Clean Up All Container and Images
docker ps -aq | xargs docker rm -f 
docker images -aq | xargs docker rmi

https://docs.docker.com/compose/install/
