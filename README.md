# baseimage [![Docker Automated build](https://img.shields.io/docker/automated/jrottenberg/ffmpeg.svg)](https://hub.docker.com/r/sheremetat/baseimage/) [![Build Status](https://travis-ci.org/sheremetat/baseimage.svg?branch=master)](https://travis-ci.org/sheremetat/baseimage)

_Minimalist docker base image to build and deploy my services and applications._

### Two images included:

1. go build image - `sheremetat/baseimage:buildgo-latest`. For build stage, includes go compiler and linters. Alpine based. Supports Go modules (with vendor folder)
2. base application image `sheremetat/baseimage:app-latest`

### Example:

```FROM sheremetat/baseimage:buildgo-latest as build-service

WORKDIR /my-service

ADD . /my-service

RUN go mod vendor
RUN go mod verify

RUN go test -mod=vendor ./...

RUN golangci-lint run --no-config --issues-exit-code=1 --exclude-use-default=false \
      --deadline=30m --disable-all --enable=govet  --enable=errcheck --enable=gocyclo \
      --enable=structcheck --enable=varcheck --enable=ineffassign --enable=deadcode \
      --enable=typecheck --enable=golint --enable=interfacer --enable=unconvert \
      --enable=dupl --enable=goconst --enable=gocyclo --enable=gosec

RUN go build -v -mod=vendor -o my-service ./


FROM sheremetat/baseimage:app-latest

WORKDIR /srv

ADD start.sh /srv/start.sh

RUN chmod +x /srv/start.sh

COPY --from=build-service /my-service/my-service /srv/

RUN chown -R app:app /srv

CMD ["/srv/start.sh"]
ENTRYPOINT ["/init.sh"]```