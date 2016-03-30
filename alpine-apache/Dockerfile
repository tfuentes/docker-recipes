FROM alpine:latest
MAINTAINER Toni Fuentes <tfuentes@ackstorm.com>

EXPOSE 80 443

RUN apk --update add apache2 apache2-proxy apache2-proxy-html && rm -rf /var/cache/apk/*

#coment to not use mpm_prefork, uncomment to use mpm_event_module and copy mpm config
RUN sed -i "s|LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|" /etc/apache2/httpd.conf &&\
sed -i "s|#LoadModule mpm_event_module modules/mod_mpm_event.so|LoadModule mpm_event_module modules/mod_mpm_event.so|" /etc/apache2/httpd.conf &&\
sed -i "s|#LoadModule slotmem_shm_module modules/mod_slotmem_shm.so|LoadModule slotmem_shm_module modules/mod_slotmem_shm.so|" /etc/apache2/httpd.conf

RUN \
    mkdir /run/apache2 && \
    chown -R apache:root /run/apache2

RUN \
    echo "Updating default apache DocumentRoot..." && \
    mkdir /var/www/html && \
    chown -R apache:apache /var/www/html && \
    # fix bug in proxy.conf and proxy-html.conf in stock apache install
    sed -i 's#/usr/lib/libxml2.so$#/usr/lib/libxml2.so.2#' /etc/apache2/conf.d/proxy-html.conf && \
    sed -i 's#/usr/lib/libxml2.so$#/usr/lib/libxml2.so.2#' /etc/apache2/conf.d/proxy.conf 

#ADD files/conf.d/* /etc/apache2/conf.d/
#ADD files/code/* /var/www/html/

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]

#CMD ["sleep","1000"]
