FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf install -y freeradius rsyslog ; \
    sed -i -e "s/auth = no/auth = yes/" /etc/raddb/radiusd.conf ;  \
    mkdir /etc/raddb/mods-config/files/userlist ; \
    systemctl enable radiusd 
COPY clients.conf /etc/raddb/clients.conf
COPY authorize /etc/raddb/mods-config/files/authorize
COPY users.login /etc/raddb/mods-config/files/userlist/users.login
COPY mabs /etc/raddb/mods-config/files/userlist/mabs
COPY rsyslog.conf /etc/rsyslog.conf
CMD [ "/usr/sbin/init" ]
