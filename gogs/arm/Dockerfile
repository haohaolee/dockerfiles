#
# Dockerfile for gogs-arm
#

FROM easypi/alpine-arm
MAINTAINER EasyPi Software Foundation

ENV GOGS_VERSION 0.9.113
ENV GOGS_CUSTOM /data/gogs

RUN apk add --no-cache bash \
                       ca-certificates \
                       curl \
                       git \
                       linux-pam \
                       openssh \
                       s6 \
                       socat \
                       tar

RUN set -xe \
    && adduser -H -D -g 'Gogs Git User' -h /data/git -s /bin/bash git \
    && passwd -u git \
    && echo "export GOGS_CUSTOM=${GOGS_CUSTOM}" >> /etc/profile

RUN set -xe \
    && curl -L https://github.com/tianon/gosu/releases/download/1.10/gosu-armhf > /usr/sbin/gosu \
    && chmod +x /usr/sbin/gosu

RUN set -xe \
    && mkdir /app/ \
    && cd /app/ \
    && curl -LO https://github.com/gogits/gogs/releases/download/v${GOGS_VERSION}/linux_arm.zip \
    && unzip linux_arm.zip \
    && rm linux_arm.zip

RUN set -xe \
    && cd /app/gogs/ \
    && curl -L https://github.com/gogits/gogs/archive/v${GOGS_VERSION}.tar.gz | tar xz --strip 1 gogs-${GOGS_VERSION}/docker \
    && mv docker/nsswitch.conf /etc/

WORKDIR /app/gogs/
VOLUME /data/

EXPOSE 22 3000

CMD ["docker/start.sh", "/bin/s6-svscan", "/app/gogs/docker/s6/"]
