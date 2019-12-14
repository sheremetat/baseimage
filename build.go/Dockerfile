FROM golang:1.13.4-alpine3.10

RUN \
    apk add --no-cache --update tzdata git bash curl && \
    cp /usr/share/zoneinfo/Europe/Kiev /etc/localtime && \
    rm -rf /var/cache/apk/*

RUN go version

ENV CGO_ENABLED=0
ENV GOOS=linux
ENV GOARCH=amd64

RUN \
    go get -u github.com/golang/dep/cmd/dep && \
    go get -u github.com/kardianos/govendor && \
    go get -u github.com/mattn/goveralls && \
    go get -u github.com/jteeuwen/go-bindata/... && \
    go get -u github.com/stretchr/testify && \
    ln -s /go/bin/gometalinter.v2 /go/bin/gometalinter && \
    gometalinter --install --force 

RUN wget -O - -q https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sh -s v1.21.0 && \
    golangci-lint --version

ADD coverage.sh /script/coverage.sh
ADD checkvendor.sh /script/checkvendor.sh

RUN \
    chmod +x /script/coverage.sh /script/checkvendor.sh && \
    ln -s /script/checkvendor.sh $GOPATH/bin/checkvendor
