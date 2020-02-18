FROM centos:centos7
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    yum -y update ; yum -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    curl -1sLf 'https://dl.cloudsmith.io/public/isc/kea-1-6/cfg/setup/bash.rpm.sh' | bash ; \
    yum -y install isc-kea ; \
    mkdir /var/log/kea ; \
    systemctl enable kea-dhcp4
COPY rsyslog.conf /etc
COPY kea_log /etc/logrotate.d
COPY kea-dhcp4.conf /etc/kea/
CMD [ "/usr/sbin/init" ]
