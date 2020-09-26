FROM alpine:3.11 AS build-env
COPY kea-hook-runscript-1.3.3.tar.gz /tmp
WORKDIR /tmp
RUN tar xvfz /tmp/kea-hook-runscript-1.3.3.tar.gz
WORKDIR /tmp/kea-hook-runscript-1.3.3/
RUN apk add curl util-linux && \
    curl -1sLf 'https://dl.cloudsmith.io/public/isc/kea-1-8/cfg/rsa/rsa.1570C74FEAD03977.key' > /etc/apk/keys/kea-1-8@isc-1570C74FEAD03977.rsa.pub && \
    curl -1sLf 'https://dl.cloudsmith.io/public/isc/kea-1-8/cfg/setup/config.alpine.txt?distro=alpine&codename=v3.11' >> /etc/apk/repositories && \
    apk add boost-dev isc-kea-dev g++ make && \
    make
# chdir=/tmp/kea-hook-runscript-1.3.3


FROM alpine:3.11
## https://cloudsmith.io/~isc/repos/kea-1-8/packages/?q=distribution%3Aalpine+AND+distribution%3Av3.11
RUN apk add --update --no-cache curl util-linux && \
    curl -1sLf 'https://dl.cloudsmith.io/public/isc/kea-1-8/cfg/rsa/rsa.1570C74FEAD03977.key' > /etc/apk/keys/kea-1-8@isc-1570C74FEAD03977.rsa.pub && \
    curl -1sLf 'https://dl.cloudsmith.io/public/isc/kea-1-8/cfg/setup/config.alpine.txt?distro=alpine&codename=v3.11' >> /etc/apk/repositories && \
    apk add --update --no-cache isc-kea-dhcp-ddns isc-kea isc-kea-dhcp4 isc-kea-ctrl-agent isc-kea-dev isc-kea-common && \
    rename 's/conf/conf.org/' /etc/kea/*.conf || \
    true
COPY --from=build-env /tmp/kea-hook-runscript-1.3.3/kea-hook-runscript.so /etc/kea
COPY kea-dhcp4.conf /etc/kea/

CMD ["/usr/sbin/kea-dhcp4", "-c", "/etc/kea/kea-dhcp4.conf"]

