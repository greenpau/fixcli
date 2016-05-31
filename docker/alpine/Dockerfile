FROM alpine:latest
MAINTAINER Paul Greenberg @greenpau

ENV PATH "$PATH:/root/fixcli/"
RUN apk update && apk --update add git python && \
    cd && git clone https://github.com/greenpau/fixcli.git && cd fixcli
CMD ["fixcli", "-h"]
