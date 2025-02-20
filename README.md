# Minimal Homer 10  🔭

This repository contains a minimal Docker Compose setup for the Homer 10. It is based on the [Homer 10](https://github.com/sipcapture/homer-docker) project, but removed the unnecessary components and simplified the setup. This setup includes the following components:

- Grafana
- ClickHouse
- Qryn
- Heplify Server
- Vector
- Node-Exporter

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
      # Replace this IP with the one for your Homer instance
      ./heplify -e -hs 67.205.155.117:9060 -m SIPRTCP -dd -zf -l info
    network_mode: host
    restart: unless-stopped
```
