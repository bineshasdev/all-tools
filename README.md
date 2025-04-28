
# Microservice Project with Docker Compose

This repository contains the Docker Compose setup for a multi-service microservice-based application. The stack integrates various tools for authentication, logging, monitoring, and tracing, leveraging Keycloak, Prometheus, Loki, Grafana, Jaeger, and more.

## Project Overview

This project uses Docker Compose to manage multiple services essential for running a cloud-native microservice application:

- **Keycloak**: Identity and Access Management (IAM) for authentication and authorization.
- **PostgreSQL**: Database for Keycloak storage.
- **Prometheus**: Monitoring and alerting toolkit.
- **Loki**: Log aggregation system.
- **Grafana**: Dashboard and visualization tool.
- **Jaeger**: Distributed tracing system.
- **Adminer**: Database management UI.
- **Consul**: Service discovery and configuration.
- **Grafana Agent**: Collects metrics and logs for monitoring.

## Docker Compose Services

### `init`
This is an init container used to change the ownership of the Loki mount directory to ensure that the Loki container has proper permissions.

### `postgres`
A PostgreSQL container for Keycloak’s database. It stores user and authentication data for Keycloak.

- **Image**: `postgres:15`
- **Ports**: Exposed internally for Keycloak to connect to.

### `adminer`
An admin interface for managing the PostgreSQL database visually.

- **Image**: `adminer`
- **Ports**: `7090:8080` (Accessible through `localhost:7090`)

### `keycloak`
A container running Keycloak for authentication and authorization.

- **Image**: `quay.io/keycloak/keycloak:latest`
- **Ports**: 
  - `9080:8080` (For accessing Keycloak Admin Console)
  - `9000:9000` (For Keycloak metrics)
- **Environment Variables**: Configures database connection and admin user details.
  
### `prometheus`
A container for Prometheus monitoring, collecting metrics from various services.

- **Image**: `bitnami/prometheus:latest`
- **Ports**: `9090:9090` (For accessing Prometheus dashboard)

### `loki`
A container for Loki, used for aggregating logs from various services.

- **Image**: `grafana/loki:latest`
- **Ports**: `3100:3100` (For accessing Loki API)
  
### `grafana`
Grafana for visualizing metrics from Prometheus and logs from Loki.

- **Image**: `grafana/grafana:latest`
- **Ports**: `3000:3000` (For accessing Grafana dashboard)

### `grafana-agent`
Collects metrics and logs from Docker containers and forwards them to Grafana and Loki.

- **Image**: `grafana/agent:v0.38.1`
- **Ports**: `12345:12345` (Exposed HTTP port)

### `jaeger`
Jaeger for distributed tracing, helping to trace requests as they flow through the system.

- **Image**: `jaegertracing/all-in-one:latest`
- **Ports**: 
  - `16686:16686` (Jaeger UI)
  - `14268:14268` (Jaeger HTTP endpoint)

### `consul`
Service discovery and configuration management tool.

- **Image**: `hashicorp/consul:latest`
- **Ports**: `8500:8500` (Consul UI accessible at `localhost:8500`)

## Environment Setup

1. **Clone the Repository**:
   ```bash
   git clone [https://github.com/your-repo/microservice-project.git](https://github.com/bineshasdev/all-tools.git)
   cd all-infra
   ```

2. **Start the Services**:
   Use Docker Compose to bring up the entire stack:
   ```bash
   docker-compose -f docker-compose.dev.yml up -d
   ```
   If you are using Podman Compose to bring up the entire stack:
   ```bash
   podman-compose -f docker-compose.dev.yml up -d
   ```
3. **Access Services**:
   - **Keycloak Admin Console**: [http://localhost:9080](http://localhost:9080)
   - **Prometheus Dashboard**: [http://localhost:9090](http://localhost:9090)
   - **Grafana Dashboard**: [http://localhost:3000](http://localhost:3000)
   - **Adminer**: [http://localhost:7090](http://localhost:7090)
   - **Jaeger UI**: [http://localhost:16686](http://localhost:16686)
   - **Consul UI**: [http://localhost:8500](http://localhost:8500)

## Volumes

The following volumes are used to persist data:

- **postgres_data**: Data for PostgreSQL.
- **prometheus-data**: Data for Prometheus.
- **loki**: Log data for Loki.
- **grafana**: Data for Grafana.
- **grafana-agent**: Data for Grafana Agent.

## Networks

All services are connected to a bridge network `devnet`, ensuring they can communicate with each other.

## Configuration Files

- **Prometheus**: `./prometheus/prometheus.yml`
- **Loki**: `./loki/config.yaml`
- **Grafana**: Dashboards and datasources configuration in `./grafana/provisioning`.
- **Keycloak**: Custom themes and configuration in `./themes` and `./config`.

## Docker Compose Tips

- To stop the services, run:
  ```bash
  docker-compose down
  podman-compose down
  
  ```
- To view logs for a specific service:
  ```bash
  docker-compose logs <service-name>
  podman-compose logs <service-name>
  ```

## Additional Information

- The `grafana-agent` collects logs and metrics from containers and sends them to Grafana and Loki for visualization and analysis.
- **Keycloak** manages user authentication and provides role-based access to the microservices.
- **Jaeger** helps in tracing distributed requests, providing insight into the application's performance and bottlenecks.

## License

Distributed under the MIT License. See `LICENSE` for more information.
