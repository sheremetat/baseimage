FROM golang:1.14.1-alpine3.11

LABEL maintainer="Taras Sheremeta <sheremetat@gmail.com>"

ENV \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64 \
    GOLANGCI=1.24.0

RUN \
    apk add --no-cache --update tzdata git bash curl && \
    cp /usr/share/zoneinfo/Europe/Kiev /etc/localtime && \
    rm -rf /var/cache/apk/*

RUN go version && \
    go get -u -v github.com/stretchr/testify
    
RUN wget -O - -q https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sh -s v${GOLANGCI} && \
    golangci-lint --version

ADD coverage.sh /script/coverage.sh

RUN \
    chmod +x /script/coverage.sh
