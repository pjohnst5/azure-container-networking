# Build azure-vnet-telemetry
FROM golang:1.16-alpine AS builder
# Build args
ARG VERSION
ARG CNI_AI_PATH
ARG CNI_AI_ID

WORKDIR /usr/local/src/azure-vnet-telemetry

# Copy the source
COPY . .


RUN CGO_ENABLED=0 go build -v -o /usr/local/bin/azure-vnet-telemetry -ldflags "-X main.version="$VERSION" -X "$CNI_AI_PATH"="$CNI_AI_ID"" -gcflags="-dwarflocationlists=true" cni/telemetry/service/*.go

# Use a minimal image as a parent image
FROM ubuntu:19.10

COPY --from=builder /usr/local/bin/azure-vnet-telemetry \
	/usr/bin/azure-vnet-telemetry

COPY --from=builder /usr/local/src/azure-vnet-telemetry/telemetry/azure-vnet-telemetry.config \
    /usr/bin/azure-vnet-telemetry.config

# Run below command by default when the container starts.
ENTRYPOINT ["/usr/bin/azure-vnet-telemetry", "-d", "/usr/bin"]