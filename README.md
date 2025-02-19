# Minimal Homer 10  🔭

This repository contains a minimal Docker Compose setup for the Homer 10. It is based on the [Homer 10](https://github.com/sipcapture/homer-docker) project, but removed the unnecessary components and simplified the setup.

## What's Inside

This setup includes the following components:

- **Grafana** provides interactive dashboards (including Call Flow, HEP Stats, and CDR Search) to visualize call and system metrics.
- **ClickHouse** stores OpenTelemetry and Zipkin trace data, which is later used for tracing and alerting.
- **Qryn** acts as the gateway for metrics, logs, and tracing data; it also supports the node graph datasource for Grafana.
- **Heplify Server** captures HEP traffic from SIP/VoIP endpoints and pushes data (e.g., HEP logs and statistics) to Loki via the Qryn service.
- **Vector** scrapes metrics from the Heplify server and system node-exporter, forwarding the data via Prometheus Remote Write.
- **Node-Exporter** provides underlying system metrics for host-level monitoring.

## How to Start the Server

First, make sure [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) are installed.

Then, run the following command in the project's root directory (where the `docker-compose.yml` is located):

```bash
docker compose up -d
```

Finally, open your browser at [http://localhost:3000](http://localhost:3000) (default credentials are `admin/admin` unless overridden).  

> Grafana is pre-configured with the necessary dashboards (e.g., Call Flow, HEP Stats, CDR Search).

## Testing with Heplify Client

To capture SIP/RTP traffic, deploy the heplify client service in your `docker-compose.yml` on the system running your SIP server (Asterisk, Kamailio, etc.):

```yaml
services:
  heplify:
    image: sipcapture/heplify:latest
    cap_add:
      - CAP_NET_ADMIN
      - CAP_NET_RAW
    command:
      ./heplify -e -hs 67.205.155.117:9060 -m SIPRTCP -dd -zf -l info
    network_mode: host
    restart: unless-stopped
```
