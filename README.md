# baseimage [![Docker Automated build](https://img.shields.io/docker/automated/jrottenberg/ffmpeg.svg)](https://hub.docker.com/r/sheremetat/baseimage/) ![](https://github.com/sheremetat/baseimage/workflows/Baseimage%20Build/badge.svg)

Minimalist docker base image to build and deploy Golang services and applications.

## Two images included:

1. go build image - `sheremetat/baseimage:buildgo-v{x.x.x}`. For build stage, includes go compiler and linters. Alpine based. `v{x.x.x}` - usually represents supportend version of Go programming language.
2. base application image `sheremetat/baseimage:app-v{x}`. Alpine based.

## Example:

```
FROM sheremetat/baseimage:buildgo-v1.18.0 as build-service

WORKDIR /build
ADD . /build

RUN go test ./...
RUN golangci-lint run

RUN go build -v -o app

FROM sheremetat/baseimage:app-v1

WORKDIR /app
COPY --from=build-service /build/app /app/svc

CMD ["./svc"]
```

## Acknowledgments

These base images are inspired by @Umputun [baseimage](https://github.com/umputun/baseimage)

## License

MIT License

Copyright (c) 2022 Taras Sheremeta

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rightsto use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOTLIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THESOFTWARE.
