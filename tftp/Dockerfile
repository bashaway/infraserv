FROM centos:centos8
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime ; \
    dnf -y update ; dnf -y install epel-release rsyslog logrotate cronie ; \
    rm -f /etc/logrotate.d/syslog ; \
    dnf -y install tftp-server xinetd ; \
    mkdir /var/log/tftpd ; \
    systemctl enable xinetd
COPY rsyslog.conf /etc
COPY tftpd_log /etc/logrotate.d
COPY tftp /etc/xinetd.d/tftp
CMD [ "/usr/sbin/init" ]
