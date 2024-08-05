
# OpenTelemetry in a Box

> [!WARNING]
> This is for development use only and should not be deployed to a production environment.

## High-Level Vision
This project aims to provide a self-contained OTel Collector and LGTM (Loki, Grafana, Tempo, and Prometheus) stack for local development testing.

## Introduction to OpenTelemetry (OTel)
[OpenTelemetry](https://opentelemetry.io/) is an observability framework for cloud-native software, providing a set of APIs, libraries, agents, and instrumentation to enable the collection of distributed traces and metrics.

## Prerequisites
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Services

### 1. [Grafana](https://grafana.com/)
Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert on, and understand your metrics no matter where they are stored.

- **Image:** grafana/grafana
- **Ports:** `3000:3000`
- **Volumes:**
  - `./grafana/datasource.yml:/etc/grafana/provisioning/datasources/datasource.yml`
  - `./grafana/dashboard.yaml:/etc/grafana/provisioning/dashboards/main.yaml`
  - `./grafana/dashboards:/var/lib/grafana/dashboards`
- **Depends on:**
  - prometheus
  - loki
  - tempo
- **Environment Variables:**
  - `GF_AUTH_ANONYMOUS_ENABLED=true`
  - `GF_AUTH_ANONYMOUS_ORG_ROLE=Admin`
  - `GF_AUTH_DISABLE_LOGIN_FORM=true`
  - `GF_FEATURE_TOGGLES_ENABLE=traceqlEditor`
- **Network:** otel

### 2. [Prometheus](https://prometheus.io/)
Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud.

- **Image:** prom/prometheus
- **Volumes:**
  - `./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml`
- **Network:** otel

### 3. [Loki](https://grafana.com/oss/loki/)
Loki is a horizontally-scalable, highly-available, multi-tenant log aggregation system inspired by Prometheus.

- **Image:** grafana/loki:3.1.0
- **Entrypoint:** `/usr/bin/loki`
- **Command:** `-config.file=/etc/loki/local-config.yaml`
- **Volumes:**
  - `./loki/loki-config.yml:/etc/loki/local-config.yaml`
  - `loki-data:/loki`
- **Network:** otel

### 4. [Tempo](https://grafana.com/oss/tempo/)
Tempo is an open-source, easy-to-use, and high-scale distributed tracing backend.

- **Image:** grafana/tempo
- **Command:** `-config.file=/etc/tempo.yaml`
- **Volumes:**
  - `./tempo/tempo-config.yml:/etc/tempo.yaml`
  - `tempo-data:/var/tempo`
- **Network:** otel

### 5. [OTel Collector](https://opentelemetry.io/docs/collector/)
The OpenTelemetry Collector offers a vendor-agnostic implementation on how to receive, process, and export telemetry data.

- **Image:** otel/opentelemetry-collector-contrib:latest
- **Ports:**
  - `8889:8889` (Prometheus exporter metrics)
  - `4317:4317` (OTLP gRPC receiver)
- **Volumes:**
  - `./otel/otel-collector-config.yml:/etc/otelcol-contrib/config.yaml`
- **Network:** otel

## Volumes
- `tempo-data`
- `loki-data`

## Networks
- `otel` (bridge)

## Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/ClarkAssociates/otel-in-a-box.git
   cd otel-in-a-box
   ```

2. Start the services:
   ```bash
   docker-compose up -d
   ```
3. Configure your application:
  - URL: `http://localhost:4317`
  - Protocol: `gRPC`

4. Access Grafana:
  - URL: `http://localhost:3000`
  - Grafana is pre-configured to use anonymous access with admin rights.

## Cleanup
To stop and remove all services and associated volumes, run:
```bash
docker-compose down -v
```

## Configuration
Configuration files for each service are provided in their respective directories:
- Grafana: `./grafana`
- Prometheus: `./prometheus`
- Loki: `./loki`
- Tempo: `./tempo`
- OTel Collector: `./otel`

These configurations can be modified to suit your local development and testing needs.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any changes or improvements.

## License
This project is licensed under the [MIT License](LICENSE.md).
