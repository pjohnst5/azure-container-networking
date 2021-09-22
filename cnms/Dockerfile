# Build cnms
FROM golang:1.16-alpine AS builder
# Build args
ARG VERSION

WORKDIR /usr/local/src/cnms

# Copy the source
COPY . .

RUN CGO_ENABLED=0 go build -v -o /usr/local/bin/azure-cnms -ldflags "-X main.version="$VERSION"" -gcflags="-dwarflocationlists=true" cnms/service/*.go

FROM ubuntu:20.04

COPY --from=builder /usr/local/bin/azure-cnms \
	/usr/bin/azure-cnms

RUN apt -y update
RUN apt-get -y upgrade
RUN apt install -y ebtables
RUN apt install -y net-tools
RUN chmod +x /usr/bin/azure-cnms
CMD ["/usr/bin/azure-cnms"]
