# data-flow-engine

A powerful real-time data processing pipeline built with Go, Kafka, PostgreSQL, MongoDB, and Docker.

## Overview

data-flow-engine provides a comprehensive solution for ingesting, processing, and visualizing high-volume data streams. The system utilizes a microservices architecture to handle data in real-time, store it efficiently across different database technologies, and expose processed data through a responsive API.

### Key Features

- High-throughput data ingestion with Apache Kafka
- Scalable data processing via Go microservices
- Dual storage strategy (PostgreSQL for analytics, MongoDB for raw events)
- Real-time data visualization capabilities
- Containerized with Docker and Docker Compose for simplified development
- Production-ready Kubernetes configurations included

## Architecture

The data-flow-engine consists of several interconnected microservices:

1. **Ingestion Service**: Accepts incoming data through HTTP/gRPC endpoints and publishes to Kafka topics
2. **Processing Service**: Consumes Kafka messages, applies business logic, and writes to databases
3. **API Service**: Provides REST endpoints for retrieving processed data
4. **Dashboard**: Simple web UI for monitoring and visualizing real-time data

## Tech Stack

- **Backend**: Go (with Gin, GORM)
- **Message Broker**: Apache Kafka
- **Databases**:
    - PostgreSQL (structured analytics data)
    - MongoDB (raw events and unstructured data)
    - Redis (caching and pub/sub)
- **Containerization**: Docker & Docker Compose
- **Orchestration**: Kubernetes (for production)
- **Monitoring**: Prometheus & Grafana

## Getting Started

### Prerequisites

- Go 1.20 or higher
- Docker and Docker Compose
- Make (optional, for using Makefile commands)

### Local Development Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/data-flow-engine.git
   cd data-flow-engine
   ```

2. Start the infrastructure services:
   ```bash
   docker-compose up -d postgres mongodb redis kafka zookeeper
   ```

3. Build and run the services:
   ```bash
   make build
   make run
   ```

   Or using Docker Compose:
   ```bash
   docker-compose up -d
   ```

4. The API service will be available at `http://localhost:8080`

### Environment Variables

Create a `.env` file in the project root with the following variables:

```
# Kafka
KAFKA_BOOTSTRAP_SERVERS=localhost:9092
KAFKA_TOPIC_RAW_DATA=raw-data
KAFKA_TOPIC_PROCESSED_DATA=processed-data

# PostgreSQL
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=dataflow

# MongoDB
MONGO_URI=mongodb://localhost:27017
MONGO_DATABASE=dataflow

# Redis
REDIS_ADDR=localhost:6379
REDIS_PASSWORD=
REDIS_DB=0

# Services
INGESTION_SERVICE_PORT=8081
PROCESSOR_SERVICE_PORT=8082
API_SERVICE_PORT=8080
```

## Project Structure

```
data-flow-engine/
‚îú‚îÄ‚îÄ cmd/                    # Application entry points
‚îÇ   ‚îú‚îÄ‚îÄ ingestion/          # Data ingestion service
‚îÇ   ‚îú‚îÄ‚îÄ processor/          # Data processing service
‚îÇ   ‚îî‚îÄ‚îÄ api/                # API service
‚îú‚îÄ‚îÄ internal/               # Private application code
‚îÇ   ‚îú‚îÄ‚îÄ models/             # Data models
‚îÇ   ‚îú‚îÄ‚îÄ kafka/              # Kafka utilities
‚îÇ   ‚îú‚îÄ‚îÄ database/           # Database interactions
‚îÇ   ‚îî‚îÄ‚îÄ config/             # Configuration handling
‚îú‚îÄ‚îÄ pkg/                    # Public libraries
‚îÇ   ‚îú‚îÄ‚îÄ logger/             # Logging utilities
‚îÇ   ‚îî‚îÄ‚îÄ metrics/            # Metrics utilities
‚îú‚îÄ‚îÄ api/                    # API definitions
‚îú‚îÄ‚îÄ deployments/            # Deployment configurations
‚îÇ   ‚îú‚îÄ‚îÄ docker/             # Docker related files
‚îÇ   ‚îî‚îÄ‚îÄ k8s/                # Kubernetes manifests
‚îú‚îÄ‚îÄ scripts/                # Helper scripts
‚îú‚îÄ‚îÄ .gitignore              # Git ignore file
‚îú‚îÄ‚îÄ README.md               # Project documentation
‚îú‚îÄ‚îÄ go.mod                  # Go modules file
‚îú‚îÄ‚îÄ docker-compose.yml      # Docker Compose configuration
‚îî‚îÄ‚îÄ Makefile                # Build and automation tasks
```

## Development Workflow

1. **Local Development**: Use Docker Compose for local development and testing
2. **Testing**: Run unit and integration tests with `make test`
3. **Building**: Build the services with `make build`
4. **Deployment**: Production deployment uses Kubernetes configurations in the `deployments/k8s` directory

## API Reference

### Ingestion API

- `POST /ingest` - Submit data for processing
  ```json
  {
    "sourceId": "sensor-123",
    "timestamp": "2025-04-11T12:00:00Z",
    "payload": {
      "temperature": 23.5,
      "humidity": 45.2
    }
  }
  ```

### Query API

- `GET /data` - Retrieve processed data with filtering options
    - Query parameters:
        - `source_id`: Filter by source ID
        - `start_time`: Filter by start timestamp (ISO format)
        - `end_time`: Filter by end timestamp (ISO format)
        - `limit`: Limit results (default: 100)
        - `offset`: Offset for pagination

## Monitoring

The system includes Prometheus and Grafana for monitoring:

- Prometheus is available at `http://localhost:9090`
- Grafana is available at `http://localhost:3000` (default credentials: admin/admin)

## Production Deployment

For production environments, Kubernetes manifests are provided in the `deployments/k8s` directory.

1. Apply the configurations:
   ```bash
   kubectl apply -f deployments/k8s/
   ```

2. For cloud deployments, adapt the configurations as needed for your specific cloud provider.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
