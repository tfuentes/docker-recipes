FROM alpine:latest

RUN apk update \
        && apk upgrade \
        && apk add haproxy \
        && rm -rf /etc/haproxy/haproxy.cfg \
        && rm -rf /var/cache/apk/*

COPY files/haproxy.cfg /etc/haproxy/
CMD [ "/usr/sbin/haproxy", "-f", "/etc/haproxy/haproxy.cfg", "-db" ]
