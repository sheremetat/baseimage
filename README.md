# baseimage [![Docker Automated build](https://img.shields.io/docker/automated/jrottenberg/ffmpeg.svg)](https://hub.docker.com/r/sheremetat/baseimage/) ![](https://github.com/sheremetat/baseimage/workflows/Baseimage%20Build/badge.svg)

Minimalist docker base image to build and deploy Golang services and applications.

### Two images included:

1. go build image - `sheremetat/baseimage:buildgo-v{x.x.x}`. For build stage, includes go compiler and linters. Alpine based. `v{x.x.x}` - usually represents supportend version of Go programming language.
2. base application image `sheremetat/baseimage:app-v{x}`. Alpine based.

### Example:

```
FROM sheremetat/baseimage:buildgo-v1.14.1 as build-service

WORKDIR /build
ADD . /build

RUN go test -mod=vendor ./...
RUN golangci-lint run

RUN go build -v -mod=vendor -o app

FROM sheremetat/baseimage:app-v1

WORKDIR /app
COPY --from=build-service /build/app /app/svc

CMD ["./svc"]
```