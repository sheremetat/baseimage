FROM golang:1.19.0-alpine3.16

LABEL maintainer="Taras Sheremeta <sheremetat@gmail.com>"

ENV \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64 \
    GOLANGCI=1.45.0

RUN \
    apk add --no-cache --update tzdata git bash curl && \
    cp /usr/share/zoneinfo/Europe/Kiev /etc/localtime && \
    rm -rf /var/cache/apk/*

RUN go version
    
RUN wget -O- -nv https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v${GOLANGCI} && \
    golangci-lint --version
