FROM centos:centos8
RUN dnf -y update ; dnf -y install tftp-server xinetd
COPY tftp /etc/xinetd.d/tftp
CMD [ "/usr/sbin/init" ]
