FROM golang:1.11.2 as base
RUN mkdir -p /go/src/parser
WORKDIR /go/src/parser
COPY . .
RUN go get -u github.com/golang/dep/...
RUN dep init
RUN dep ensure -vendor-only
RUN apt-get update
RUN apt-get install poppler-utils -y

FROM base as builder
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest as final
WORKDIR /root/
RUN apk update && apk add poppler-utils && rm -rf /var/cache/apk/*
COPY --from=builder /go/src/parser/main .

