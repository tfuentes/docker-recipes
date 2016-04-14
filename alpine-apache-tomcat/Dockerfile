FROM ackstorm/alpine-java-7:latest
MAINTAINER Toni Fuentes <tfuentes@ackstorm.com>

#APACHE
RUN apk --update add ca-certificates apache2 apache2-proxy apache2-proxy-html && rm -rf /var/cache/apk/*

#coment to not use mpm_prefork, uncomment to use mpm_event_module and copy mpm config
RUN sed -i "s|LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|" /etc/apache2/httpd.conf && \
sed -i "s|#LoadModule mpm_event_module modules/mod_mpm_event.so|LoadModule mpm_event_module modules/mod_mpm_event.so|" /etc/apache2/httpd.conf && \
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

# SUPERVISOR
RUN apk --update add supervisor

#TOMCAT 
ENV TOMCAT_VERSION_MAJOR 7
ENV TOMCAT_VERSION_FULL 7.0.55

# Download and install
RUN apk --update add curl && \
  mkdir /opt && \
  curl -kLsS https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_VERSION_MAJOR}/v${TOMCAT_VERSION_FULL}/bin/apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz \
  | gunzip -c - | tar -xf - -C /opt && \
  ln -s /opt/apache-tomcat-${TOMCAT_VERSION_FULL} /opt/tomcat && \
  rm -rf /opt/tomcat/webapps/examples /opt/tomcat/webapps/docs

# Configuration
#ADD files/conf/tomcat/tomcat-users.xml /opt/tomcat/conf/
#ADD files/conf/tomcat/server.xml /opt/tomcat/conf/
#ADD files/conf/tomcat/logging.properties /opt/tomcat/conf/
ADD files/conf/apache2/sites.conf /etc/apache2/conf.d/
ADD files/conf/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

#applications
#ADD files/app/ /opt/tomcat/webapps

# Set environment
ENV CATALINA_HOME /opt/tomcat

EXPOSE 80

CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]
