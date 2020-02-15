FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf install -y squid ; \
    sed -i -e "s/http_port 3128/http_port 8080/" /etc/squid/squid.conf ;  \
    systemctl enable squid ; \
    dnf -y install rsyslog
COPY rsyslog.conf /etc
CMD [ "/usr/sbin/init" ]

