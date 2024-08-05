
# [OpenTelemetry](https://opentelemetry.io/) in a Box

> [!WARNING]
> This is for development use only and should not be deployed to a production environment.

## High-Level Vision
This project aims to provide a self-contained OTel Collector and LGTM (Loki, Grafana, Tempo, and Prometheus) stack for local development testing.

## Prerequisites
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Services
| Service                                                    | Use                                                                                                                                                                            | Ports                                                                             |
|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| [OTel Collector](https://opentelemetry.io/docs/collector/) | The OpenTelemetry Collector offers a vendor-agnostic implementation on how to receive, process, and export telemetry data.                                                     | `8889:8889` - Prometheus exporter metrics<br />`4317:4317` - OTLP `gRPC` receiver |
| [Grafana](https://grafana.com/)                            | Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert on, and understand your metrics no matter where they are stored. | `3000:3000` - The main user interface                                             |
| [Prometheus](https://prometheus.io/)                       | Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud.                                                                           |                                                                                   |
| [Loki](https://grafana.com/oss/loki/)                      | Loki is a horizontally-scalable, highly-available, multi-tenant log aggregation system inspired by Prometheus.                                                                 |                                                                                   |
| [Tempo](https://grafana.com/oss/tempo/)                    | Tempo is an open-source, easy-to-use, and high-scale distributed tracing backend.                                                                                              |                                                                                   |

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
