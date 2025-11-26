# Apache Iceberg

Apache Iceberg learning environment with Spark, MinIO, and REST catalog.

## Repository Structure

```
apacheiceberg/
├── .env                    # Environment variables for sensitive credentials
├── docker-compose.yml      # Docker Compose configuration for all services
├── README.md              # This file
├── warehouse/             # Iceberg warehouse storage (created at runtime)
├── notebooks/             # Jupyter notebooks for Iceberg experimentation
└── data/                  # Data files for ingestion and processing
```

## Services

This repository sets up a complete Apache Iceberg stack with the following services:

- **spark-iceberg**: Spark environment with Iceberg support and Jupyter notebook interface
- **iceberg-rest**: Apache Iceberg REST catalog for metadata management
- **minio**: S3-compatible object storage for Iceberg data files
- **mc**: MinIO client for initializing the warehouse bucket

## Prerequisites

- Docker
- Docker Compose

## Getting Started

### 1. Environment Setup

The `.env` file contains sensitive credentials. Default values are provided, but you should update them for production use:

```bash
AWS_ACCESS_KEY_ID=admin
AWS_SECRET_ACCESS_KEY=password
AWS_REGION=us-east-1
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=password
```

**Important**: Never commit the `.env` file with real credentials to version control.

### 2. Start the Services

```bash
docker-compose up -d
```

This will start all services in the background.

### 3. Prepare NBA Dataset

The repository includes NBA game data in a compressed archive. Extract it before running the notebooks:

```bash
# Extract the NBA data from the archive
unzip data/archive.zip -d data/nba

# Verify the extraction
ls data/nba
# Expected files: games.csv, games_details.csv, players.csv, ranking.csv, teams.csv
```

**Note**: The extracted files are already in `.gitignore` to avoid committing large CSV files.

### 4. Access the Services

- **Jupyter Notebook**: http://localhost:8888
- **Spark UI**: http://localhost:8080
- **MinIO Console**: http://localhost:9001
- **Iceberg REST Catalog**: http://localhost:8181

### 5. Working with Iceberg

Access the Jupyter notebook interface at http://localhost:8888 to start working with Apache Iceberg tables. The Spark session is pre-configured to use the Iceberg REST catalog and MinIO for storage.

### 6. Stop the Services

```bash
docker-compose down
```

To remove volumes as well:

```bash
docker-compose down -v
```

## Configuration

### Environment Variables

All sensitive credentials are managed through the `.env` file:

- **AWS_ACCESS_KEY_ID** / **AWS_SECRET_ACCESS_KEY**: Credentials for S3/MinIO access
- **AWS_REGION**: AWS region configuration
- **MINIO_ROOT_USER** / **MINIO_ROOT_PASSWORD**: MinIO admin credentials

### Ports

- `8888`: Jupyter Notebook
- `8080`: Spark Master UI
- `9000`: MinIO API
- `9001`: MinIO Console
- `8181`: Iceberg REST Catalog
- `10000`, `10001`: Spark Thrift Server
- `4040`, `4041`: Spark Application UI

## Data Persistence

- `./warehouse`: Local directory mounted for Iceberg warehouse files
- `./notebooks`: Local directory for Jupyter notebooks
- `./data`: Local directory for data files to be processed

## Troubleshooting

### Services not starting

Check logs with:

```bash
docker-compose logs -f [service-name]
```

### MinIO bucket not created

The `mc` service automatically creates the warehouse bucket. Check its logs:

```bash
docker-compose logs mc
```

### Connection issues

Ensure all services are healthy:

```bash
docker-compose ps
```
