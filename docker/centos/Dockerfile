FROM centos:latest
MAINTAINER Paul Greenberg @greenpau

ENV PATH "$PATH:/root/fixcli/"
RUN yum -y install git && \
    cd && git clone https://github.com/greenpau/fixcli.git && cd fixcli
CMD ["fixcli", "-h"]
