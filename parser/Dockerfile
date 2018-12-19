FROM golang:1.11.2 as base
RUN mkdir -p /go/src/parser
WORKDIR /go/src/parser
COPY . .
RUN go get -u github.com/golang/dep/...
RUN dep init
RUN dep ensure -vendor-only

FROM base as builder
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest as final
WORKDIR /root/
COPY --from=builder /go/src/parser/main .

