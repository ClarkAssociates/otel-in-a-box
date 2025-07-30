
# [OpenTelemetry](https://opentelemetry.io/) in a Box

> [!WARNING]
> This is for development use only and should not be deployed to a production environment.

## High-Level Vision
This project aims to provide a self-contained OTel Collector and LGTM (Loki, Grafana, Tempo, and Prometheus) stack for local development testing.

## Prerequisites
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

## Services
| Service                                                    | Use                                                                                                                                                                            | Locally Exposed Ports                                                             |
|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| [OTel Collector](https://opentelemetry.io/docs/collector/) | The OpenTelemetry Collector offers a vendor-agnostic implementation on how to receive, process, and export telemetry data.                                                     | `8889:8889` - Prometheus exporter metrics<br />`4317:4317` - OTLP `gRPC` receiver |
| [Grafana](https://grafana.com/)                            | Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert on, and understand your metrics no matter where they are stored. | `3000:3000` - Grafana user interface                                              |
| [Prometheus](https://prometheus.io/)                       | Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud.                                                                           | `9090:9090` - Prometheus user interface                                           |
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
   - URL: [http://localhost:4317](http://localhost:4317)
   - Protocol: `gRPC`
4. Access Grafana:
   - URL: [http://localhost:3000](http://localhost:3000)
   - Grafana is pre-configured to use anonymous access with admin rights.
5. (Optional) Access Prometheus:
   - URL: [http://localhost:9090](http://localhost:9090)

## Cleanup
To stop and remove all services and associated volumes, run:
```bash
docker-compose down -v
```
## Service Configuration
Configuration files for each service are provided in their respective directories:
- Grafana: [`./grafana`](./grafana)
- Prometheus: [`./prometheus`](./prometheus)
- Loki: [`./loki`](./loki)
- Tempo: [`./tempo`](./tempo)
- OTel Collector: [`./otel`](./otel)

These configurations can be modified to suit your local development and testing needs.

## FAQ

### Resolving a Port Conflict with Docker Compose
If you encounter a port conflict because a service is already running on one of the ports exposed by this `docker-compose.yml` file, follow these steps to remap the port:

1) Choose a New Port:
   - Select a new, unused port on your local system.
   - This new port number should be between `1024` and `65535`.
2) Edit the `docker-compose.yml` File:
   - Open the `docker-compose.yml` file in a text editor.
   - Locate the ports section for the service that has the conflicting port.
   - Find the mapping that looks like `XXXX:XXXX` (where `XXXX` represents the port numbers).
   - Change the left side of the mapping (the host port) to your new port value.

Example:
If the original `docker-compose.yml` file has a port mapping like this:

```yaml
ports:
  - "8080:80"
```
And `8080` is the conflicting port, and you choose `9090` as the new port, update it to:
```yaml
ports:
  - "9090:80"
```

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any changes or improvements.

## License
This project is licensed under the [Apache 2 License](LICENSE).
