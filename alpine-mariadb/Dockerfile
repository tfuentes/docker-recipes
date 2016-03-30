FROM alpine:latest
MAINTAINER tfuentes - https://github.com/tfuentes

RUN apk --update add mysql mysql-client pwgen && rm -f /var/cache/apk/* && \
echo "Success"

ADD files/run.sh /scripts/run.sh
RUN mkdir /scripts/pre-exec.d && \
mkdir /scripts/pre-init.d && \
chmod -R 755 /scripts

EXPOSE 3306
# WORKDIR /app

VOLUME ["/var/lib/mysql"]
VOLUME ["/etc/mysql"]

ENTRYPOINT ["/scripts/run.sh"]
