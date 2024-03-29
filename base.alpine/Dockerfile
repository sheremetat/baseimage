FROM alpine:3.16

LABEL maintainer="Taras Sheremeta <sheremetat@gmail.com>"

ENV \
    TERM=xterm-color           \
    DINIT_VERSION=1.2.1        \
    TIME_ZONE=Europe/Kiev      \
    MYUSER=app                 \
    MYUID=1001                 \
    DOCKER_GID=999         

COPY files/scripts/nop.sh /usr/bin/nop.sh
COPY files/scripts/app.sh /usr/bin/app.sh

RUN \
    chmod +x /usr/bin/nop.sh /usr/bin/app.sh && \
    apk add --no-cache --update su-exec tzdata curl openssl && \
    ln -s /sbin/su-exec /usr/local/bin/gosu && \
    mkdir -p /home/$MYUSER && \
    adduser -s /bin/sh -D -u $MYUID $MYUSER && chown -R $MYUSER:$MYUSER /home/$MYUSER && \
    delgroup ping && addgroup -g 998 ping && \
    addgroup -g ${DOCKER_GID} docker && addgroup ${MYUSER} docker && \
    mkdir -p /srv && chown -R $MYUSER:$MYUSER /srv && \
    cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && date && \
    wget -q https://github.com/Yelp/dumb-init/releases/download/v${DINIT_VERSION}/dumb-init_${DINIT_VERSION}_amd64 -O /sbin/dinit && \
    chmod +x /sbin/dinit && \
    rm -rf /var/cache/apk/*
    