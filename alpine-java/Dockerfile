FROM alpine:latest

MAINTAINER Toni Fuentes <toni.fuentes@ackstorm.com>

ENV JAVA_HOME=/usr/lib/jvm/default-jvm
ENV JAVA_VERSION 7

RUN apk add --no-cache openjdk${JAVA_VERSION} java-cacerts ca-certificates && \
    ln -sf "${JAVA_HOME}/bin/"* "/usr/bin/"

ADD files/cacerts /usr/lib/jvm/java-1.${JAVA_VERSION}-openjdk/jre/lib/security/cacerts

CMD ["java","-version"]
