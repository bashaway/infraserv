FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    dnf -y install unbound ; \
    mkdir /var/log/unbound ; chown unbound.unbound /var/log/unbound ; \
    systemctl enable unbound
COPY rsyslog.conf /etc
COPY unbound_log /etc/logrotate.d
COPY unbound.conf /etc/unbound
CMD [ "/usr/sbin/init" ]
