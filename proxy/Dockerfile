FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    dnf -y install squid ; \
    sed -i -e "s/http_port 3128/http_port 8080/" /etc/squid/squid.conf ;  \
    systemctl enable squid
COPY rsyslog.conf /etc
COPY squid_log /etc/logrotate.d
CMD [ "/usr/sbin/init" ]
