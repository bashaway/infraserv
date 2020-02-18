FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    dnf -y install freeradius ; \
    sed -i -e "s/auth = no/auth = yes/" /etc/raddb/radiusd.conf ;  \
    mkdir /etc/raddb/mods-config/files/userlist ; \
    systemctl enable radiusd 
COPY rsyslog.conf /etc
COPY radius_log /etc/logrotate.d
COPY clients.conf /etc/raddb/clients.conf
COPY authorize /etc/raddb/mods-config/files/authorize
COPY users.login /etc/raddb/mods-config/files/userlist/users.login
COPY mabs /etc/raddb/mods-config/files/userlist/mabs
CMD [ "/usr/sbin/init" ]
