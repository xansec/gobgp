FROM golang AS builder

ENV GOPATH=/usr/local/go/src
COPY . .
RUN rm $GOPATH/go.mod
RUN go mod download
WORKDIR ./cmd/gobgpd
RUN go build

FROM ubuntu

#RUN apt update -y && DEBIAN_FRONTEND=noninteractive apt install -y strace net-tools
RUN mkdir -p /go/cmd/gobgpd
RUN mkdir /go/mayhem
COPY --from=builder /go/cmd/gobgpd/gobgpd /gobgpd
COPY --from=builder /go/mayhem/gobgpd-nopeer.conf /gobgpd-nopeer.conf
COPY ./mayhem/hosts /etc/hosts
COPY ./mayhem/resolv.conf /etc/resolv.conf

CMD ["/gobgpd", "-f", "/gobgpd-nopeer.conf"]

