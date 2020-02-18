FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    mkdir /var/log/remotelog
COPY rsyslog.conf /etc
COPY remotelog /etc/logrotate.d
CMD [ "/usr/sbin/init" ]
