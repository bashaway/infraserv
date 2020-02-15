FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf install -y rsyslog cronie logrotate ; \
    mkdir /var/log/remotelog ;\
    rm -f /etc/logrotate.d/syslog
COPY rsyslog.conf /etc/rsyslog.conf
COPY remotelog /etc/logrotate.d
CMD [ "/usr/sbin/init" ]
