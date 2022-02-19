FROM golang:1.17-alpine3.15 AS builder

RUN mkdir /app

WORKDIR /app

RUN go get github.com/letsencrypt/unbound_exporter
RUN go install github.com/letsencrypt/unbound_exporter@latest
RUN which unbound_exporter

FROM alpine:3.15
RUN apk --no-cache add ca-certificates

WORKDIR /app
COPY --from=builder /go/bin/unbound_exporter /app/unbound_exporter
RUN chown -R nobody /app && chmod +x /app/unbound_exporter

USER nobody
# In case this needs to change for different options
# Usage of /app/unbound_exporter:
#  -unbound.ca string
#    	Unbound server certificate. (default "/etc/unbound/unbound_server.pem")
#  -unbound.cert string
#    	Unbound client certificate. (default "/etc/unbound/unbound_control.pem")
#  -unbound.host string
#    	Unix or TCP address of Unbound control socket. (default "tcp://localhost:8953")
#  -unbound.key string
#    	Unbound client key. (default "/etc/unbound/unbound_control.key")
#  -web.listen-address string
#    	Address to listen on for web interface and telemetry. (default ":9167")
#  -web.telemetry-path string
#    	Path under which to expose metrics. (default "/metrics")
ENTRYPOINT ["/app/unbound_exporter"]
