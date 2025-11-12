# Project Plan: Real-Time Pharmaceutical Batch Quality & Compliance Analytics Platform

## 🎯 **Your 8-Week Learning Journey**

This document is your **roadmap** to building a production-level data engineering project. Follow this action plan step-by-step, and you'll gain hands-on experience with the entire modern data stack.

---

## **📅 Action Plan: Week-by-Week Breakdown**

### **Phase 1: Foundation Setup (Week 1)**
**Goal:** Set up your development environment with Docker from day one and understand the problem you're solving.

---

#### **Day 1-2: Understand the Problem Domain**

**What You're Doing:**
Learn the pharmaceutical manufacturing context so you understand WHY the data pipeline matters. This helps you design realistic data schemas and KPIs.

**Tasks:**
1. Read the [Problem Statement](#problem-statement) below
2. Research pharmaceutical manufacturing:
   - What is a batch? (a fixed quantity of drug product made in one production run)
   - What is GMP? (Good Manufacturing Practice - regulatory requirement)
   - What sensors are used? (temperature, humidity, pressure)
   - What is FDA compliance? (ensures drug safety and efficacy)

3. Sketch your data flows - create a document listing:
   ```
   My Data Sources (What systems produce data):
   - Manufacturing Execution System (MES) → Batch status events
   - Equipment Sensors → Temperature, humidity readings every 5 minutes
   - Laboratory Information System (LIMS) → Quality test results
   
   My KPIs to Track (What metrics matter):
   1. Batch Pass Rate (% of batches passing quality tests)
   2. Average Batch Yield (% of expected output achieved)
   3. Equipment Utilization (% of time equipment is running)
   4. Temperature Compliance (% of time within spec)
   5. Average Investigation Time (days to close deviations)
   ```

**Deliverable:**
- [ ] Document with 5 KPIs and data sources (save in `docs/` or as a note)

---

#### **Day 3-4: Environment Setup**

**What You're Doing:**
Set up a local development environment with Docker so you can run everything consistently. Docker ensures that what runs on your laptop will also run on the cloud and other machines.

**Prerequisites Check:**
Run these commands to see what you have:

```powershell
# Check Docker
docker --version

# Check Docker Compose
docker-compose --version

# Check Python
python --version

# Check Git
git --version
```

**Tasks:**

##### **A. Install Docker Desktop (if not already installed)**

1. Download from: https://www.docker.com/products/docker-desktop/
2. Install and restart your computer
3. Verify: Open PowerShell and run:
   ```powershell
   docker run hello-world
   ```
   You should see a success message

##### **B. Set up Python Virtual Environment**

**Why:** Isolate your Python dependencies so they don't conflict with other projects.

```powershell
# Navigate to your project
cd "c:\Users\abhay.ahirkar\OneDrive - TRIDIAGONAL.AI PRIVATE LIMITED\learning\DE\Sensorlytics"

# Create virtual environment
python -m venv venv

# Activate it
.\venv\Scripts\Activate.ps1

# If you get execution policy error, run:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Upgrade pip
python -m pip install --upgrade pip
```

##### **C. Update `.gitignore` File**

**Why:** Prevent accidentally committing secrets, logs, and dependencies.

Add these entries to your existing `.gitignore`:

```
# Environment variables
.env
.env.local

# Docker volumes (local data)
data/
logs/

# IDE and OS files
.DS_Store
.vscode/
.idea/
*.swp

# Python cache
__pycache__/
*.pyc
*.pyo
*.pyd
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Virtual environments
venv/
ENV/
env/

# Jupyter
.ipynb_checkpoints/

# Mac
.DS_Store

# Logs
*.log
logs/

# Temp files
*.tmp
*.bak
*~

# Terraform
*.tfstate
*.tfstate.*
.terraform/
.terraform.lock.hcl
```

##### **D. Create `.env.example` File**

**What This File Is:**
A template for environment variables. Users copy this to `.env` and fill in their values. It's checked into Git, but `.env` is not.

**File: `.env.example`**

```bash
# ======================
# GCP Configuration
# ======================
GCP_PROJECT_ID=your-gcp-project-id
GCP_REGION=us-central1
GCS_BUCKET_NAME=pharma-data-lake-dev
GCS_RAW_DATA_PATH=raw/
GCS_PROCESSED_DATA_PATH=processed/

# ======================
# BigQuery Configuration
# ======================
BQ_DATASET_NAME=pharma_analytics
BQ_LOCATION=US

# ======================
# Kafka Configuration
# ======================
KAFKA_BOOTSTRAP_SERVERS=kafka:9092
KAFKA_BROKER_COUNT=1
ZOOKEEPER_PORT=2181
SCHEMA_REGISTRY_URL=http://schema-registry:8081
SCHEMA_REGISTRY_PORT=8081

# ======================
# Docker Configuration
# ======================
DOCKER_NETWORK=pharma-network

# ======================
# Python Configuration
# ======================
PYTHONUNBUFFERED=1

# ======================
# Logging Configuration
# ======================
LOG_LEVEL=INFO
LOG_DIR=./logs
```

**Instructions for users:**
```powershell
# Users copy this file
cp .env.example .env

# Then edit .env and add their actual values
# .env is in .gitignore, so it won't be committed
```

**Deliverables:**
- [ ] Python virtual environment created and activated
- [ ] `.gitignore` updated with Docker and Python entries
- [ ] `.env.example` file created
- [ ] Git status clean (only new files not committed yet)

---

#### **Day 5: Directory Structure**

**What You're Doing:**
Create the folder structure that will organize your code. Start with Phase 1 folders only; add more later.

**Initial Directory Creation:**

```powershell
# Navigate to project
cd "c:\Users\abhay.ahirkar\OneDrive - TRIDIAGONAL.AI PRIVATE LIMITED\learning\DE\Sensorlytics"

# Create Phase 1 directories
mkdir streaming_pipeline
mkdir docker
mkdir scripts
mkdir logs
mkdir docs

# Create subdirectories
mkdir docker\kafka
mkdir scripts\setup
mkdir docs\architecture
```

**Your Project Structure (after Day 5):**

```
Sensorlytics/
├── streaming_pipeline/          # Real-time data ingestion (Phase 2)
│   ├── producers/               # Will add later
│   ├── consumers/               # Will add later
│   └── schemas/                 # Will add later
│
├── docker/                      # All Docker configs
│   ├── kafka/                   # Kafka container setup
│   ├── spark/                   # Will add in Phase 4
│   ├── airflow/                 # Will add in Phase 6
│   └── metabase/                # Will add in Phase 7
│
├── scripts/                     # Utility scripts
│   ├── setup/                   # Setup scripts
│   └── ...
│
├── logs/                        # Application logs (gitignored)
│
├── docs/                        # Documentation
│   ├── architecture/            # Diagrams
│   └── ...
│
├── .env.example                 # Environment template
├── .gitignore                   # Git ignore rules
├── README.md                    # Project overview
├── plan.md                      # This file
└── LICENSE                      # License
```

**Deliverable:**
- [ ] Directory structure created as shown above

---

#### **Day 6-7: Docker Compose Basics**

**What You're Doing:**
Create a `docker-compose.yml` file that defines and starts Kafka + Zookeeper using Docker. This is your first microservices architecture!

**Why Docker Compose?**
- Defines all services in one file
- Networks containers automatically
- Start/stop everything with one command
- Reproduce environment exactly on any machine

**File: `docker-compose.yml`**

**Location:** Project root directory

**Content:**

```yaml
version: '3.8'

# Define volumes for data persistence
volumes:
  zookeeper_data:
  zookeeper_logs:
  kafka_data:

# Define network for inter-container communication
networks:
  pharma_network:
    driver: bridge

services:
  # ======================
  # Zookeeper
  # ======================
  # Purpose: Manages Kafka cluster state and coordination
  # Port: 2181 (internal)
  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
      ZOOKEEPER_SYNC_LIMIT: 5
      ZOOKEEPER_INIT_LIMIT: 10
    ports:
      - "2181:2181"  # Zookeeper client port
    volumes:
      - zookeeper_data:/var/lib/zookeeper/data
      - zookeeper_logs:/var/lib/zookeeper/log
    networks:
      - pharma_network
    healthcheck:
      test: ["CMD", "echo", "ruok", "|", "nc", "localhost", "2181"]
      interval: 10s
      timeout: 5s
      retries: 5
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  # ======================
  # Kafka Broker
  # ======================
  # Purpose: Message broker for real-time data streaming
  # Ports: 9092 (internal), 9093 (external)
  kafka:
    image: confluentinc/cp-kafka:7.5.0
    container_name: kafka
    depends_on:
      zookeeper:
        condition: service_healthy
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:9093
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
      KAFKA_DELETE_TOPIC_ENABLE: "true"
      KAFKA_LOG_RETENTION_HOURS: 168
      KAFKA_LOG_SEGMENT_BYTES: 1073741824
    ports:
      - "9092:9092"  # Internal communication
      - "9093:9093"  # External communication (from host)
    volumes:
      - kafka_data:/var/lib/kafka/data
    networks:
      - pharma_network
    healthcheck:
      test: ["CMD", "kafka-broker-api-versions.sh", "--bootstrap-server", "kafka:9092"]
      interval: 10s
      timeout: 5s
      retries: 5
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  # ======================
  # Schema Registry
  # ======================
  # Purpose: Manages Avro schemas for data validation
  # Port: 8081
  schema-registry:
    image: confluentinc/cp-schema-registry:7.5.0
    container_name: schema-registry
    depends_on:
      kafka:
        condition: service_healthy
    environment:
      SCHEMA_REGISTRY_HOST_NAME: schema-registry
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: kafka:9092
      SCHEMA_REGISTRY_LISTENERS: http://0.0.0.0:8081
      SCHEMA_REGISTRY_KAFKASTORE_TOPIC: _schemas
      SCHEMA_REGISTRY_DEBUG: "false"
    ports:
      - "8081:8081"  # Schema Registry API
    networks:
      - pharma_network
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8081/"]
      interval: 10s
      timeout: 5s
      retries: 5
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  # ======================
  # Kafka UI (Optional - for visualization)
  # ======================
  # Purpose: Web UI to explore Kafka topics and messages
  # Port: 8080
  kafka-ui:
    image: provectuslabs/kafka-ui:latest
    container_name: kafka-ui
    depends_on:
      - kafka
      - schema-registry
    environment:
      KAFKA_CLUSTERS_0_NAME: pharma-cluster
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
      KAFKA_CLUSTERS_0_ZOOKEEPER: zookeeper:2181
      KAFKA_CLUSTERS_0_SCHEMAREGISTRY: http://schema-registry:8081
    ports:
      - "8080:8080"  # Kafka UI dashboard
    networks:
      - pharma_network
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

**File: `requirements.txt`**

**Purpose:** Python dependencies for local development and testing

**Location:** Project root directory

**Content:**

```
# Core Data Engineering
confluent-kafka==2.3.0          # Kafka producer/consumer
avro-python3==1.10.2            # Avro serialization
pydantic==2.5.0                 # Data validation

# Cloud & Storage
google-cloud-storage==2.10.0    # GCS access
google-cloud-bigquery==3.13.0   # BigQuery access

# Data Processing
pandas==2.1.3                   # Data manipulation
numpy==1.26.2                   # Numerical computing

# Utilities
python-dotenv==1.0.0            # Load .env files
requests==2.31.0                # HTTP requests
pytz==2023.3                    # Timezone handling

# Logging & Monitoring
python-json-logger==2.0.7       # JSON logging

# Development & Testing
pytest==7.4.3                   # Unit testing
pytest-cov==4.1.0               # Coverage reporting
black==23.12.0                  # Code formatting
flake8==6.1.0                   # Linting
mypy==1.7.1                     # Type checking

# Documentation
sphinx==7.2.6                   # Documentation generation
sphinx-rtd-theme==2.0.0         # ReadTheDocs theme
```

**Tasks for Day 6-7:**

1. **Create docker-compose.yml file**
   ```powershell
   # Create the file with content shown above
   # Save to: Sensorlytics\docker-compose.yml
   ```

2. **Create requirements.txt file**
   ```powershell
   # Create the file with content shown above
   # Save to: Sensorlytics\requirements.txt
   ```

3. **Install Python dependencies**
   ```powershell
   # Make sure venv is activated
   pip install -r requirements.txt
   ```

4. **Start Docker services**
   ```powershell
   # Navigate to project root
   cd "c:\Users\abhay.ahirkar\OneDrive - TRIDIAGONAL.AI PRIVATE LIMITED\learning\DE\Sensorlytics"
   
   # Start all services
   docker-compose up -d
   
   # Check status
   docker-compose ps
   
   # View logs
   docker-compose logs -f  # Follow logs in real-time
   
   # View Kafka logs specifically
   docker-compose logs kafka
   ```

5. **Verify Kafka is running**
   ```powershell
   # List Kafka topics (should be empty initially)
   docker-compose exec kafka kafka-topics.sh --list --bootstrap-server localhost:9092
   
   # Create a test topic
   docker-compose exec kafka kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
   
   # List topics again (should show test-topic)
   docker-compose exec kafka kafka-topics.sh --list --bootstrap-server localhost:9092
   ```

6. **Verify Schema Registry**
   ```powershell
   # Check Schema Registry health
   curl http://localhost:8081/
   
   # Should return: {"id":1,"version":"7.5.0","kafkaClusterId":"..."}
   ```

7. **Access Kafka UI**
   - Open browser: http://localhost:8080
   - See Kafka cluster, topics, and messages visually

8. **Stop services (when done testing)**
   ```powershell
   docker-compose down
   
   # To also remove volumes (clean slate)
   docker-compose down -v
   ```

**Understanding Each Component:**

| Component | Purpose | Port | Use Case |
|-----------|---------|------|----------|
| **Zookeeper** | Coordinates Kafka cluster | 2181 | Internal Kafka management |
| **Kafka Broker** | Message broker | 9092-9093 | Stores and routes messages |
| **Schema Registry** | Validates message schemas | 8081 | Ensures data consistency |
| **Kafka UI** | Web dashboard | 8080 | Visualize topics and messages |

**Files Created This Week:**

```
Sensorlytics/
├── docker-compose.yml           ← NEW: Defines all services
├── requirements.txt              ← NEW: Python dependencies
├── .env.example                  ← NEW: Environment template
├── .gitignore                    ← UPDATED: Added Docker/Python entries
├── streaming_pipeline/           ← NEW: Directory
├── docker/                       ← NEW: Directory
│   └── kafka/                    ← NEW: Directory
├── scripts/                      ← NEW: Directory
├── logs/                         ← NEW: Directory (gitignored)
├── docs/                         ← NEW: Directory
├── plan.md                       ← EXISTING
├── README.md                     ← EXISTING
└── LICENSE                       ← EXISTING
```

**Deliverables:**
- [ ] `docker-compose.yml` created and working
- [ ] `requirements.txt` created with core dependencies
- [ ] `.env.example` created with configuration template
- [ ] `.gitignore` updated for Docker/Python
- [ ] All Docker services running successfully
- [ ] Kafka topic creation verified
- [ ] Schema Registry responding to requests
- [ ] Kafka UI accessible at http://localhost:8080
- [ ] All code committed to Git

**Common Issues & Fixes:**

| Issue | Cause | Fix |
|-------|-------|-----|
| "docker: command not found" | Docker not installed or not in PATH | Install Docker Desktop and restart |
| Port already in use (9092, 8081, etc.) | Another service using port | Run `docker-compose down`, check with `netstat -ano` |
| "schema-registry unhealthy" | Kafka not ready yet | Wait a few seconds, check `docker-compose logs schema-registry` |
| Kafka can't connect to Zookeeper | Network issue | Verify `depends_on` in docker-compose.yml |
| Permission denied writing to volumes | Docker permissions | Ensure Docker daemon is running with proper permissions |

**Next Steps (Preview of Week 2):**
Once this is running, you'll:
1. Create Python producer to generate batch status data
2. Publish messages to Kafka topic
3. Create consumer to read messages
4. Test end-to-end data flow

**Resources:**
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Confluent Kafka Docker Images](https://github.com/confluentinc/cp-docker-images)
- [Kafka Documentation](https://kafka.apache.org/documentation/)

---

### **Phase 2: Streaming Pipeline (Week 2)**
**Goal:** Build a working Kafka producer-consumer pipeline with schema validation. Learn how to design data schemas and create real-time data generators.

---

#### **Day 1-2: Design Data Schemas**

**What You're Doing:**
Define the structure of data that will flow through Kafka. Avro schemas ensure data quality and compatibility as your code evolves.

**Why Avro?**
- **Efficient:** Binary format = smaller messages, faster transmission
- **Typed:** Catches errors early (wrong data type = rejected)
- **Versioning:** Schema Registry handles backward compatibility
- **Language-agnostic:** Works with Python, Java, Go, etc.

**Tasks:**

##### **A. Create Directory Structure**

```powershell
# Navigate to project
cd "c:\Users\abhay.ahirkar\OneDrive - TRIDIAGONAL.AI PRIVATE LIMITED\learning\DE\Sensorlytics"

# Create streaming pipeline directories
mkdir streaming_pipeline\schemas
mkdir streaming_pipeline\producers
mkdir streaming_pipeline\consumers

# Create data directories (for local testing)
mkdir data\batch_status
mkdir data\sensor_data
mkdir data\lab_results
```

##### **B. Create Batch Status Schema**

**File:** `streaming_pipeline/schemas/batch_status.avsc`

**Purpose:** Defines structure of batch production events (e.g., batch started, batch completed)

**Content:**

```json
{
  "type": "record",
  "name": "BatchStatus",
  "namespace": "com.pharma.production",
  "doc": "Batch production status event from MES (Manufacturing Execution System)",
  "fields": [
    {
      "name": "batch_id",
      "type": "string",
      "doc": "Unique identifier for the batch (e.g., BATCH-2025-001)"
    },
    {
      "name": "product_id",
      "type": "string",
      "doc": "Product code being manufactured (e.g., PROD-500MG)"
    },
    {
      "name": "status",
      "type": {
        "type": "enum",
        "name": "BatchStatusEnum",
        "symbols": ["QUEUED", "IN_PROGRESS", "COMPLETED", "FAILED", "ON_HOLD"]
      },
      "doc": "Current status of the batch"
    },
    {
      "name": "equipment_id",
      "type": "string",
      "doc": "Equipment identifier where batch is being produced"
    },
    {
      "name": "batch_size",
      "type": "double",
      "doc": "Total batch size in kg"
    },
    {
      "name": "start_time",
      "type": {
        "type": "long",
        "logicalType": "timestamp-millis"
      },
      "doc": "Batch start timestamp (milliseconds since epoch)"
    },
    {
      "name": "end_time",
      "type": [
        "null",
        {
          "type": "long",
          "logicalType": "timestamp-millis"
        }
      ],
      "default": null,
      "doc": "Batch end timestamp (null if still in progress)"
    },
    {
      "name": "operator_id",
      "type": "string",
      "doc": "ID of operator managing the batch"
    },
    {
      "name": "event_timestamp",
      "type": {
        "type": "long",
        "logicalType": "timestamp-millis"
      },
      "doc": "When this event was generated"
    }
  ]
}
```

##### **C. Create Sensor Data Schema**

**File:** `streaming_pipeline/schemas/sensor_data.avsc`

**Purpose:** Defines structure of equipment sensor readings (temperature, humidity, pressure)

**Content:**

```json
{
  "type": "record",
  "name": "SensorReading",
  "namespace": "com.pharma.equipment",
  "doc": "Real-time sensor data from manufacturing equipment",
  "fields": [
    {
      "name": "sensor_id",
      "type": "string",
      "doc": "Unique sensor identifier (e.g., TEMP-EQUIP-001)"
    },
    {
      "name": "batch_id",
      "type": "string",
      "doc": "Associated batch ID"
    },
    {
      "name": "equipment_id",
      "type": "string",
      "doc": "Equipment where sensor is installed"
    },
    {
      "name": "sensor_type",
      "type": {
        "type": "enum",
        "name": "SensorTypeEnum",
        "symbols": ["TEMPERATURE", "HUMIDITY", "PRESSURE", "VIBRATION"]
      },
      "doc": "Type of sensor"
    },
    {
      "name": "value",
      "type": "double",
      "doc": "Sensor reading value"
    },
    {
      "name": "unit",
      "type": "string",
      "doc": "Unit of measurement (e.g., Celsius, %, PSI)"
    },
    {
      "name": "specification_min",
      "type": "double",
      "doc": "Minimum acceptable value"
    },
    {
      "name": "specification_max",
      "type": "double",
      "doc": "Maximum acceptable value"
    },
    {
      "name": "is_out_of_spec",
      "type": "boolean",
      "doc": "True if value violates specification"
    },
    {
      "name": "reading_timestamp",
      "type": {
        "type": "long",
        "logicalType": "timestamp-millis"
      },
      "doc": "When sensor reading was taken"
    }
  ]
}
```

##### **D. Create Lab Results Schema**

**File:** `streaming_pipeline/schemas/lab_results.avsc`

**Purpose:** Defines structure of quality control test results from LIMS

**Content:**

```json
{
  "type": "record",
  "name": "LabResult",
  "namespace": "com.pharma.quality",
  "doc": "Quality control test results from LIMS (Laboratory Information Management System)",
  "fields": [
    {
      "name": "test_id",
      "type": "string",
      "doc": "Unique test identifier"
    },
    {
      "name": "batch_id",
      "type": "string",
      "doc": "Associated batch ID"
    },
    {
      "name": "test_name",
      "type": "string",
      "doc": "Name of quality test (e.g., 'Dissolution', 'Weight Uniformity')"
    },
    {
      "name": "test_result",
      "type": "double",
      "doc": "Numerical result of the test"
    },
    {
      "name": "unit",
      "type": "string",
      "doc": "Unit of measurement"
    },
    {
      "name": "specification_min",
      "type": "double",
      "doc": "Minimum acceptable result"
    },
    {
      "name": "specification_max",
      "type": "double",
      "doc": "Maximum acceptable result"
    },
    {
      "name": "pass_fail",
      "type": {
        "type": "enum",
        "name": "TestResultEnum",
        "symbols": ["PASS", "FAIL", "CONDITIONAL"]
      },
      "doc": "Pass/Fail status"
    },
    {
      "name": "analyst_id",
      "type": "string",
      "doc": "ID of analyst who performed test"
    },
    {
      "name": "test_timestamp",
      "type": {
        "type": "long",
        "logicalType": "timestamp-millis"
      },
      "doc": "When test was performed"
    }
  ]
}
```

**Deliverable:**
- [ ] All 3 Avro schema files created in `streaming_pipeline/schemas/`

---

#### **Day 3-4: Build Your First Producer**

**What You're Doing:**
Create a Python script that generates realistic pharmaceutical batch data and sends it to Kafka. This simulates the MES (Manufacturing Execution System) sending batch status events.

**Prerequisites:**
- Docker services running (`docker-compose up -d`)
- Python virtual environment activated
- Python packages installed from `requirements.txt`

**File:** `streaming_pipeline/producers/batch_status_producer.py`

**Content:**

```python
"""
Batch Status Producer
Simulates Manufacturing Execution System (MES) events.
Generates batch lifecycle events: QUEUED → IN_PROGRESS → COMPLETED
"""

import json
import logging
import time
from datetime import datetime, timedelta
from typing import Dict, Any
import random
import uuid
from confluent_kafka import Producer
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer
from confluent_kafka.serialization import StringSerializer
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# ==================== Configuration ====================
KAFKA_BOOTSTRAP_SERVERS = os.getenv("KAFKA_BOOTSTRAP_SERVERS", "localhost:9092")
SCHEMA_REGISTRY_URL = os.getenv("SCHEMA_REGISTRY_URL", "http://localhost:8081")
KAFKA_TOPIC = "pharma.batch_status"

# ==================== Logging Setup ====================
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

# ==================== Schema Registry Client ====================
def get_schema_registry_client():
    """Initialize Schema Registry client"""
    return SchemaRegistryClient({"url": SCHEMA_REGISTRY_URL})

# ==================== Schema Loading ====================
def load_schema(schema_path: str) -> str:
    """Load Avro schema from file"""
    with open(schema_path, 'r') as f:
        return f.read()

# ==================== Kafka Producer Setup ====================
def create_kafka_producer():
    """Create Kafka producer with Avro serialization"""
    
    # Load schema
    schema_str = load_schema("streaming_pipeline/schemas/batch_status.avsc")
    
    # Initialize Schema Registry client
    sr_client = get_schema_registry_client()
    
    # Create Avro serializer
    avro_serializer = AvroSerializer(sr_client, schema_str)
    
    # Create producer config
    producer_config = {
        "bootstrap.servers": KAFKA_BOOTSTRAP_SERVERS,
        "key.serializer": StringSerializer("utf_8"),
        "value.serializer": avro_serializer,
        "acks": "all",  # Wait for all replicas
        "retries": 3,
        "socket.keepalive.enable": True,
    }
    
    producer = Producer(producer_config)
    return producer

# ==================== Data Generation ====================
def generate_batch_events():
    """
    Generate batch events with realistic lifecycle:
    1. QUEUED - batch is waiting to start
    2. IN_PROGRESS - batch is being produced
    3. COMPLETED - batch finished successfully
    """
    
    # Sample data
    products = ["PROD-500MG", "PROD-250MG", "PROD-100MG"]
    equipment_ids = ["EQUIP-001", "EQUIP-002", "EQUIP-003"]
    operators = ["OP-101", "OP-102", "OP-103", "OP-104"]
    batch_sizes = [100.0, 150.0, 200.0, 250.0]  # kg
    
    while True:
        try:
            # Generate batch
            batch_id = f"BATCH-{datetime.utcnow().strftime('%Y%m%d')}-{random.randint(1000, 9999)}"
            product_id = random.choice(products)
            equipment_id = random.choice(equipment_ids)
            operator_id = random.choice(operators)
            batch_size = random.choice(batch_sizes)
            start_time = int(datetime.utcnow().timestamp() * 1000)
            
            # Generate batch states in sequence
            states = ["QUEUED", "IN_PROGRESS", "COMPLETED"]
            
            for idx, state in enumerate(states):
                event = {
                    "batch_id": batch_id,
                    "product_id": product_id,
                    "status": state,
                    "equipment_id": equipment_id,
                    "batch_size": batch_size,
                    "start_time": start_time,
                    "end_time": int((datetime.utcnow() + timedelta(hours=2)).timestamp() * 1000) 
                                if state == "COMPLETED" else None,
                    "operator_id": operator_id,
                    "event_timestamp": int(datetime.utcnow().timestamp() * 1000),
                }
                
                logger.info(f"Generated event: {batch_id} -> {state}")
                yield event
                
                # Wait between state transitions (simulate real timing)
                time.sleep(5)  # 5 seconds between events for demo purposes
            
            # Wait before generating next batch
            time.sleep(10)
            
        except Exception as e:
            logger.error(f"Error generating batch event: {e}")
            time.sleep(5)

# ==================== Message Callback ====================
def delivery_report(err, msg):
    """Callback for when message is delivered or fails"""
    if err is not None:
        logger.error(f"Message delivery failed: {err}")
    else:
        logger.info(f"Message delivered to {msg.topic()} [{msg.partition()}]")

# ==================== Main Producer Logic ====================
def run_producer():
    """Main producer loop"""
    producer = create_kafka_producer()
    
    logger.info(f"Starting Batch Status Producer...")
    logger.info(f"Kafka Bootstrap Servers: {KAFKA_BOOTSTRAP_SERVERS}")
    logger.info(f"Schema Registry URL: {SCHEMA_REGISTRY_URL}")
    logger.info(f"Topic: {KAFKA_TOPIC}")
    
    try:
        for event in generate_batch_events():
            try:
                # Produce message
                producer.produce(
                    topic=KAFKA_TOPIC,
                    key=event["batch_id"],
                    value=event,
                    on_delivery=delivery_report
                )
                
                # Flush periodically to ensure delivery
                producer.flush(1)
                
            except Exception as e:
                logger.error(f"Error producing message: {e}")
    
    except KeyboardInterrupt:
        logger.info("Producer interrupted by user")
    
    finally:
        # Ensure all messages are sent before closing
        producer.flush(10)
        producer.close()
        logger.info("Producer closed")

if __name__ == "__main__":
    run_producer()
```

**What This Producer Does:**
1. Connects to Kafka and Schema Registry
2. Loads the batch status Avro schema
3. Generates realistic batch events: QUEUED → IN_PROGRESS → COMPLETED
4. Sends events to `pharma.batch_status` topic
5. Uses Avro serialization for data validation

**Deliverable:**
- [ ] `batch_status_producer.py` created in `streaming_pipeline/producers/`

---

#### **Day 5: Add More Producers**

**What You're Doing:**
Expand to simulate all three data sources: batch events, sensor data, and lab results.

**File:** `streaming_pipeline/producers/sensor_data_producer.py`

**Content:**

```python
"""
Sensor Data Producer
Simulates equipment sensors generating readings every 5 minutes.
Generates temperature, humidity, pressure readings.
"""

import json
import logging
import time
from datetime import datetime
import random
from confluent_kafka import Producer
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer
from confluent_kafka.serialization import StringSerializer
import os
from dotenv import load_dotenv

load_dotenv()

# ==================== Configuration ====================
KAFKA_BOOTSTRAP_SERVERS = os.getenv("KAFKA_BOOTSTRAP_SERVERS", "localhost:9092")
SCHEMA_REGISTRY_URL = os.getenv("SCHEMA_REGISTRY_URL", "http://localhost:8081")
KAFKA_TOPIC = "pharma.sensor_data"

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# ==================== Sensor Specifications ====================
SENSOR_SPECS = {
    "TEMPERATURE": {
        "min": 20.0,
        "max": 25.0,
        "unit": "Celsius",
        "variation": 0.5,  # Max change per reading
    },
    "HUMIDITY": {
        "min": 40.0,
        "max": 60.0,
        "unit": "%",
        "variation": 2.0,
    },
    "PRESSURE": {
        "min": 1.0,
        "max": 2.0,
        "unit": "bar",
        "variation": 0.1,
    },
}

def get_schema_registry_client():
    return SchemaRegistryClient({"url": SCHEMA_REGISTRY_URL})

def load_schema(schema_path: str) -> str:
    with open(schema_path, 'r') as f:
        return f.read()

def create_kafka_producer():
    schema_str = load_schema("streaming_pipeline/schemas/sensor_data.avsc")
    sr_client = get_schema_registry_client()
    avro_serializer = AvroSerializer(sr_client, schema_str)
    
    producer_config = {
        "bootstrap.servers": KAFKA_BOOTSTRAP_SERVERS,
        "key.serializer": StringSerializer("utf_8"),
        "value.serializer": avro_serializer,
        "acks": "all",
        "retries": 3,
    }
    
    return Producer(producer_config)

def generate_sensor_readings():
    """Generate realistic sensor data"""
    sensor_ids = [f"SENSOR-{i:03d}" for i in range(1, 10)]
    equipment_ids = ["EQUIP-001", "EQUIP-002", "EQUIP-003"]
    sensor_types = ["TEMPERATURE", "HUMIDITY", "PRESSURE"]
    
    # Initialize current values (will vary over time)
    current_values = {sensor: random.uniform(22.0, 24.0) for sensor in sensor_ids}
    
    batch_id = "BATCH-2025-0001"
    counter = 0
    
    while True:
        try:
            sensor_id = random.choice(sensor_ids)
            equipment_id = random.choice(equipment_ids)
            sensor_type = random.choice(sensor_types)
            spec = SENSOR_SPECS[sensor_type]
            
            # Simulate value drift (realistic sensor behavior)
            current_value = current_values.get(sensor_id, spec["min"] + (spec["max"] - spec["min"]) / 2)
            current_value += random.uniform(-spec["variation"], spec["variation"])
            current_value = max(spec["min"] - 2, min(spec["max"] + 2, current_value))  # Allow slight out-of-spec
            current_values[sensor_id] = current_value
            
            is_out_of_spec = current_value < spec["min"] or current_value > spec["max"]
            
            event = {
                "sensor_id": sensor_id,
                "batch_id": batch_id,
                "equipment_id": equipment_id,
                "sensor_type": sensor_type,
                "value": round(current_value, 2),
                "unit": spec["unit"],
                "specification_min": spec["min"],
                "specification_max": spec["max"],
                "is_out_of_spec": is_out_of_spec,
                "reading_timestamp": int(datetime.utcnow().timestamp() * 1000),
            }
            
            if counter % 10 == 0:  # Log every 10th reading
                logger.info(f"Generated sensor event: {sensor_id} - {sensor_type} = {current_value} {spec['unit']}")
            
            counter += 1
            yield event
            
            # Sensors read every 5 minutes (simulate with 2-second interval for demo)
            time.sleep(2)
            
        except Exception as e:
            logger.error(f"Error generating sensor event: {e}")
            time.sleep(5)

def delivery_report(err, msg):
    if err is not None:
        logger.error(f"Message delivery failed: {err}")

def run_producer():
    producer = create_kafka_producer()
    
    logger.info(f"Starting Sensor Data Producer...")
    logger.info(f"Topic: {KAFKA_TOPIC}")
    
    try:
        for event in generate_sensor_readings():
            try:
                producer.produce(
                    topic=KAFKA_TOPIC,
                    key=event["sensor_id"],
                    value=event,
                    on_delivery=delivery_report
                )
                producer.flush(1)
            except Exception as e:
                logger.error(f"Error producing message: {e}")
    
    except KeyboardInterrupt:
        logger.info("Producer interrupted")
    
    finally:
        producer.flush(10)
        producer.close()

if __name__ == "__main__":
    run_producer()
```

**File:** `streaming_pipeline/producers/lab_results_producer.py`

**Content:**

```python
"""
Lab Results Producer
Simulates LIMS (Laboratory Information Management System) quality test results.
Generates test results every 2 hours per batch.
"""

import json
import logging
import time
from datetime import datetime
import random
from confluent_kafka import Producer
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer
from confluent_kafka.serialization import StringSerializer
import os
from dotenv import load_dotenv

load_dotenv()

# ==================== Configuration ====================
KAFKA_BOOTSTRAP_SERVERS = os.getenv("KAFKA_BOOTSTRAP_SERVERS", "localhost:9092")
SCHEMA_REGISTRY_URL = os.getenv("SCHEMA_REGISTRY_URL", "http://localhost:8081")
KAFKA_TOPIC = "pharma.lab_results"

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# ==================== Quality Tests Definition ====================
QUALITY_TESTS = {
    "DISSOLUTION": {
        "min": 85.0,
        "max": 110.0,
        "unit": "%",
    },
    "WEIGHT_UNIFORMITY": {
        "min": -5.0,
        "max": 5.0,
        "unit": "%",
    },
    "HARDNESS": {
        "min": 50.0,
        "max": 120.0,
        "unit": "N",
    },
    "MOISTURE": {
        "min": 0.0,
        "max": 3.0,
        "unit": "%",
    },
}

def get_schema_registry_client():
    return SchemaRegistryClient({"url": SCHEMA_REGISTRY_URL})

def load_schema(schema_path: str) -> str:
    with open(schema_path, 'r') as f:
        return f.read()

def create_kafka_producer():
    schema_str = load_schema("streaming_pipeline/schemas/lab_results.avsc")
    sr_client = get_schema_registry_client()
    avro_serializer = AvroSerializer(sr_client, schema_str)
    
    producer_config = {
        "bootstrap.servers": KAFKA_BOOTSTRAP_SERVERS,
        "key.serializer": StringSerializer("utf_8"),
        "value.serializer": avro_serializer,
        "acks": "all",
        "retries": 3,
    }
    
    return Producer(producer_config)

def generate_lab_results():
    """Generate quality test results"""
    analysts = ["ANALYST-001", "ANALYST-002", "ANALYST-003"]
    batch_id = "BATCH-2025-0001"
    test_counter = 0
    
    while True:
        try:
            test_name = random.choice(list(QUALITY_TESTS.keys()))
            spec = QUALITY_TESTS[test_name]
            
            # Generate result with 90% pass rate
            if random.random() < 0.9:
                # Generate passing result
                test_result = random.uniform(spec["min"], spec["max"])
                pass_fail = "PASS"
            else:
                # Generate failing result
                if random.choice([True, False]):
                    test_result = spec["min"] - random.uniform(1, 10)
                else:
                    test_result = spec["max"] + random.uniform(1, 10)
                pass_fail = "FAIL"
            
            event = {
                "test_id": f"TEST-{datetime.utcnow().strftime('%Y%m%d%H%M%S')}-{test_counter:04d}",
                "batch_id": batch_id,
                "test_name": test_name,
                "test_result": round(test_result, 2),
                "unit": spec["unit"],
                "specification_min": spec["min"],
                "specification_max": spec["max"],
                "pass_fail": pass_fail,
                "analyst_id": random.choice(analysts),
                "test_timestamp": int(datetime.utcnow().timestamp() * 1000),
            }
            
            logger.info(f"Generated test result: {test_name} = {test_result} {spec['unit']} [{pass_fail}]")
            test_counter += 1
            yield event
            
            # Lab tests every 2 hours (simulate with 10-second interval for demo)
            time.sleep(10)
            
        except Exception as e:
            logger.error(f"Error generating lab result: {e}")
            time.sleep(5)

def delivery_report(err, msg):
    if err is not None:
        logger.error(f"Message delivery failed: {err}")

def run_producer():
    producer = create_kafka_producer()
    
    logger.info(f"Starting Lab Results Producer...")
    logger.info(f"Topic: {KAFKA_TOPIC}")
    
    try:
        for event in generate_lab_results():
            try:
                producer.produce(
                    topic=KAFKA_TOPIC,
                    key=event["test_id"],
                    value=event,
                    on_delivery=delivery_report
                )
                producer.flush(1)
            except Exception as e:
                logger.error(f"Error producing message: {e}")
    
    except KeyboardInterrupt:
        logger.info("Producer interrupted")
    
    finally:
        producer.flush(10)
        producer.close()

if __name__ == "__main__":
    run_producer()
```

**Deliverables:**
- [ ] `sensor_data_producer.py` created
- [ ] `lab_results_producer.py` created

---

#### **Day 6-7: Build Your First Consumer**

**What You're Doing:**
Create a consumer that reads messages from Kafka and writes them to local JSON files. This simulates the data lake.

**File:** `streaming_pipeline/consumers/kafka_to_local_consumer.py`

**Content:**

```python
"""
Kafka to Local File Consumer
Reads messages from Kafka topics and writes them to local JSON files.
Partitions data by date and topic.
"""

import json
import logging
import os
from datetime import datetime
from pathlib import Path
from confluent_kafka import Consumer, KafkaError
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroDeserializer
from confluent_kafka.serialization import StringDeserializer
from dotenv import load_dotenv

load_dotenv()

# ==================== Configuration ====================
KAFKA_BOOTSTRAP_SERVERS = os.getenv("KAFKA_BOOTSTRAP_SERVERS", "localhost:9092")
SCHEMA_REGISTRY_URL = os.getenv("SCHEMA_REGISTRY_URL", "http://localhost:8081")
TOPICS = ["pharma.batch_status", "pharma.sensor_data", "pharma.lab_results"]
OUTPUT_BASE_DIR = "./data"

# ==================== Logging Setup ====================
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

# ==================== Kafka Consumer Setup ====================
def create_kafka_consumer():
    """Create Kafka consumer with Avro deserialization"""
    
    sr_client = SchemaRegistryClient({"url": SCHEMA_REGISTRY_URL})
    avro_deserializer = AvroDeserializer(sr_client)
    
    consumer_config = {
        "bootstrap.servers": KAFKA_BOOTSTRAP_SERVERS,
        "group.id": "pharma-local-consumer",
        "key.deserializer": StringDeserializer("utf_8"),
        "value.deserializer": avro_deserializer,
        "auto.offset.reset": "earliest",  # Start from beginning
        "enable.auto.commit": True,
        "auto.commit.interval.ms": 5000,
    }
    
    return Consumer(consumer_config)

# ==================== File I/O Functions ====================
def get_output_path(topic: str, timestamp: datetime) -> str:
    """
    Generate output file path.
    Pattern: ./data/batch_status/2025-11-10/batch_status-2025-11-10-14.jsonl
    Each hour gets one file.
    """
    topic_name = topic.split(".")[-1]  # Extract topic name after dot
    date_str = timestamp.strftime("%Y-%m-%d")
    hour_str = timestamp.strftime("%H")
    
    output_dir = os.path.join(OUTPUT_BASE_DIR, topic_name, date_str)
    Path(output_dir).mkdir(parents=True, exist_ok=True)
    
    filename = f"{topic_name}-{date_str}-{hour_str}.jsonl"
    return os.path.join(output_dir, filename)

def write_message_to_file(topic: str, message: dict):
    """Write message to JSONL file"""
    try:
        timestamp = datetime.utcnow()
        output_path = get_output_path(topic, timestamp)
        
        # Write as JSONL (one JSON object per line)
        with open(output_path, 'a') as f:
            f.write(json.dumps(message) + "\n")
        
        logger.info(f"Wrote message to {output_path}")
        
    except Exception as e:
        logger.error(f"Error writing to file: {e}")

# ==================== Consumer Logic ====================
def run_consumer():
    """Main consumer loop"""
    consumer = create_kafka_consumer()
    consumer.subscribe(TOPICS)
    
    logger.info(f"Starting Kafka Consumer...")
    logger.info(f"Bootstrap Servers: {KAFKA_BOOTSTRAP_SERVERS}")
    logger.info(f"Schema Registry: {SCHEMA_REGISTRY_URL}")
    logger.info(f"Topics: {TOPICS}")
    logger.info(f"Output Directory: {OUTPUT_BASE_DIR}")
    
    try:
        while True:
            msg = consumer.poll(timeout=1.0)
            
            if msg is None:
                continue
            
            if msg.error():
                if msg.error().code() == KafkaError._PARTITION_EOF:
                    continue
                else:
                    logger.error(f"Consumer error: {msg.error()}")
                    break
            
            try:
                # Deserialize message
                topic = msg.topic()
                key = msg.key().decode("utf-8") if msg.key() else None
                value = msg.value()  # Already deserialized by AvroDeserializer
                
                # Add metadata
                message_with_metadata = {
                    "kafka_topic": topic,
                    "kafka_partition": msg.partition(),
                    "kafka_offset": msg.offset(),
                    "kafka_timestamp": msg.timestamp()[1],  # Message timestamp in ms
                    "consumed_at": int(datetime.utcnow().timestamp() * 1000),
                    **value  # Spread the actual message data
                }
                
                # Write to file
                write_message_to_file(topic, message_with_metadata)
                
            except Exception as e:
                logger.error(f"Error processing message: {e}")
    
    except KeyboardInterrupt:
        logger.info("Consumer interrupted by user")
    
    finally:
        consumer.close()
        logger.info("Consumer closed")

if __name__ == "__main__":
    run_consumer()
```

**What This Consumer Does:**
1. Subscribes to all three Kafka topics
2. Receives Avro-serialized messages
3. Partitions messages by topic and date
4. Writes to JSONL files (one JSON per line)
5. Tracks Kafka metadata (topic, partition, offset)

**Testing the Pipeline:**

In separate terminals, run these commands:

```powershell
# Terminal 1: Start Batch Status Producer
cd "c:\Users\abhay.ahirkar\OneDrive - TRIDIAGONAL.AI PRIVATE LIMITED\learning\DE\Sensorlytics"
python .\venv\Scripts\activate.ps1
python streaming_pipeline/producers/batch_status_producer.py

# Terminal 2: Start Sensor Data Producer
python streaming_pipeline/producers/sensor_data_producer.py

# Terminal 3: Start Lab Results Producer
python streaming_pipeline/producers/lab_results_producer.py

# Terminal 4: Start Consumer
python streaming_pipeline/consumers/kafka_to_local_consumer.py

# Terminal 5 (Optional): View files being created
Get-ChildItem -Recurse ./data
```

**Verify Data:**

```powershell
# Check files created
Get-ChildItem -Path ./data -Recurse

# View contents of a file
Get-Content ./data/batch_status/2025-11-10/batch_status-2025-11-10-14.jsonl -Head 5

# Count messages in a file
(Get-Content ./data/batch_status/2025-11-10/batch_status-2025-11-10-14.jsonl | Measure-Object -Line).Lines
```

**File Structure Created:**

```
Sensorlytics/
├── streaming_pipeline/
│   ├── schemas/
│   │   ├── batch_status.avsc
│   │   ├── sensor_data.avsc
│   │   └── lab_results.avsc
│   ├── producers/
│   │   ├── batch_status_producer.py
│   │   ├── sensor_data_producer.py
│   │   └── lab_results_producer.py
│   └── consumers/
│       └── kafka_to_local_consumer.py
│
├── data/  (created during execution)
│   ├── batch_status/
│   │   └── 2025-11-10/
│   │       └── batch_status-2025-11-10-14.jsonl
│   ├── sensor_data/
│   │   └── 2025-11-10/
│   │       └── sensor_data-2025-11-10-14.jsonl
│   └── lab_results/
│       └── 2025-11-10/
│           └── lab_results-2025-11-10-14.jsonl
```

**Learning Checkpoints:**

As you work through this, ask yourself:

1. **Schema Design:** Why did we use `timestamp-millis` as a logical type instead of storing as string?
2. **Avro vs JSON:** Why is Avro better than JSON for streaming?
3. **Partitioning:** Why partition by date and hour? What are the benefits?
4. **Exactly-once:** Is our consumer exactly-once, at-least-once, or at-most-once delivery? (Hint: Check `enable.auto.commit`)
5. **Offset Management:** What happens if the consumer crashes? Where does it resume from?
6. **Schema Evolution:** What would happen if you added a new field to a schema?

**Deliverables:**
- [ ] All 3 producers created and tested
- [ ] Consumer created and reading all topics
- [ ] Data files being written to `./data/` directory
- [ ] End-to-end test: Producer → Kafka → Consumer → Files
- [ ] Able to view data files with correct JSON format
- [ ] Commit all code to Git

**Common Issues & Fixes:**

| Issue | Cause | Fix |
|-------|-------|-----|
| "Schema not found" | Schema file path incorrect | Verify schema files are in `streaming_pipeline/schemas/` |
| Consumer receives no messages | Consumer started after producer | Use `auto.offset.reset: earliest` to start from beginning |
| "Connection refused" on localhost:9092 | Docker services not running | Run `docker-compose up -d` and wait for health checks |
| Files not being created | Output directory doesn't exist | Consumer creates it automatically, check permissions |
| Duplicate messages | Auto-commit interval too long | Normal behavior with `at-least-once` semantics |

**Next Steps (Preview of Week 3):**
In Phase 3, you'll:
1. Set up GCP project
2. Create Terraform for GCS bucket
3. Modify consumer to write to GCS instead of local files
4. Add partitioning by batch_id in GCS paths

**Resources:**
- [Confluent Kafka Python Client](https://docs.confluent.io/kafka-clients/python/current/overview.html)
- [Avro Specification](https://avro.apache.org/docs/current/spec.html)
- [Kafka Topic Design Best Practices](https://kafka.apache.org/documentation/#topicconfigs)

---

### **Phase 3: Cloud Integration with GCP & Terraform (Week 3)**
**Goal:** Move from local files to Google Cloud Storage, automate infrastructure provisioning with Terraform.

---

#### **Day 1-2: GCP Project Setup**

**What You're Doing:**
Set up a Google Cloud Platform project and service account that will allow your applications to read/write to GCS and BigQuery. This is critical for cloud data engineering.

**Tasks:**

##### **A. Create GCP Project**

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click project dropdown → Select "New Project"
3. Name: `pharma-data-pipeline`
4. Click "Create"
5. Wait for project to be created

##### **B. Enable Required APIs**

```powershell
# Install Google Cloud CLI
# Download from: https://cloud.google.com/sdk/docs/install

# Authenticate
gcloud auth login

# Set project
gcloud config set project YOUR_PROJECT_ID

# Enable APIs
gcloud services enable storage-api.googleapis.com
gcloud services enable bigquery.googleapis.com
gcloud services enable compute.googleapis.com
```

##### **C. Create Service Account**

**Why:** A service account is like a "robot user" that your applications can use to access GCP resources.

```powershell
# Create service account
gcloud iam service-accounts create pharma-pipeline `
  --display-name="Pharma Data Pipeline Service Account"

# Get your project ID
$PROJECT_ID = gcloud config get-value project

# Grant necessary roles
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:pharma-pipeline@${PROJECT_ID}.iam.gserviceaccount.com" `
  --role="roles/storage.admin"

gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:pharma-pipeline@${PROJECT_ID}.iam.gserviceaccount.com" `
  --role="roles/bigquery.admin"

# Create and download JSON key
gcloud iam service-accounts keys create ./gcp-creds.json `
  --iam-account=pharma-pipeline@${PROJECT_ID}.iam.gserviceaccount.com
```

**Important:** Keep `gcp-creds.json` secure! Add to `.gitignore`:

```bash
# Add to .gitignore
gcp-creds.json
*.json  # All JSON key files
```

##### **D. Set Up Billing Alerts**

```powershell
# Check billing account
gcloud billing accounts list

# Set up budget alert (via console recommended)
# Go to: Billing → Budgets and alerts → Create budget
# Set limit: $10 USD (free tier limit)
```

**Deliverable:**
- [ ] GCP project created
- [ ] APIs enabled
- [ ] Service account created with roles
- [ ] `gcp-creds.json` downloaded and in `.gitignore`
- [ ] Billing alerts configured

---

#### **Day 3-4: Terraform Infrastructure Setup**

**What You're Doing:**
Write Infrastructure as Code (IaC) using Terraform to define cloud resources. This means version control for infrastructure!

**Directory Structure:**

```powershell
# Create terraform directory structure
mkdir infrastructure\terraform\modules\gcs
mkdir infrastructure\terraform\modules\bigquery
mkdir infrastructure\terraform\modules\iam
mkdir infrastructure\terraform\environments\dev
mkdir infrastructure\terraform\environments\prod
```

**File:** `infrastructure/terraform/variables.tf`

**Purpose:** Define input variables for your infrastructure

**Content:**

```hcl
variable "project_id" {
  description = "GCP Project ID"
  type        = string
}

variable "region" {
  description = "GCP Region"
  type        = string
  default     = "us-central1"
}

variable "environment" {
  description = "Environment name (dev, prod)"
  type        = string
  default     = "dev"
}

variable "bucket_name" {
  description = "GCS Bucket name for data lake"
  type        = string
}

variable "dataset_id" {
  description = "BigQuery dataset ID"
  type        = string
  default     = "pharma_analytics"
}

variable "service_account_email" {
  description = "Service account email for pipeline"
  type        = string
}
```

**File:** `infrastructure/terraform/modules/gcs/main.tf`

**Purpose:** Terraform module for GCS bucket

**Content:**

```hcl
# GCS Bucket for data lake
resource "google_storage_bucket" "data_lake" {
  name          = var.bucket_name
  project       = var.project_id
  location      = var.region
  force_destroy = true  # Allow terraform destroy to delete bucket

  versioning {
    enabled = true
  }

  uniform_bucket_level_access = true

  # Lifecycle rule: move old data to cold storage after 30 days
  lifecycle_rule {
    condition {
      age = 30
    }
    action {
      storage_class = "COLDLINE"
      type          = "SetStorageClass"
    }
  }

  # Lifecycle rule: delete data after 90 days (optional)
  lifecycle_rule {
    condition {
      age = 90
    }
    action {
      type = "Delete"
    }
  }

  labels = {
    environment = var.environment
    project     = "pharma-pipeline"
  }
}

# GCS Folder structure
resource "google_storage_bucket_object" "raw_data_folder" {
  name   = "raw/"
  bucket = google_storage_bucket.data_lake.name
  content = " "
}

resource "google_storage_bucket_object" "processed_data_folder" {
  name   = "processed/"
  bucket = google_storage_bucket.data_lake.name
  content = " "
}
```

**File:** `infrastructure/terraform/modules/gcs/variables.tf`

```hcl
variable "bucket_name" {
  description = "GCS bucket name"
  type        = string
}

variable "project_id" {
  description = "GCP Project ID"
  type        = string
}

variable "region" {
  description = "GCP Region"
  type        = string
}

variable "environment" {
  description = "Environment"
  type        = string
}
```

**File:** `infrastructure/terraform/modules/gcs/outputs.tf`

```hcl
output "bucket_name" {
  value       = google_storage_bucket.data_lake.name
  description = "Name of the created GCS bucket"
}

output "bucket_url" {
  value       = google_storage_bucket.data_lake.url
  description = "URL of the GCS bucket"
}
```

**File:** `infrastructure/terraform/modules/bigquery/main.tf`

**Purpose:** Terraform module for BigQuery dataset

**Content:**

```hcl
# BigQuery Dataset
resource "google_bigquery_dataset" "pharma_analytics" {
  dataset_id    = var.dataset_id
  project       = var.project_id
  location      = var.location
  friendly_name = "Pharma Analytics"
  description   = "Analytics dataset for pharmaceutical production"

  access {
    role          = "OWNER"
    user_by_email = var.service_account_email
  }

  access {
    role          = "READER"
    special_group = "projectReaders"
  }

  labels = {
    environment = var.environment
    project     = "pharma-pipeline"
  }

  # Default expiration for tables: 90 days
  default_table_expiration_ms = 7776000000
}

# BigQuery Table: Batch Production (raw)
resource "google_bigquery_table" "raw_batch_production" {
  dataset_id = google_bigquery_dataset.pharma_analytics.dataset_id
  table_id   = "raw_batch_production"
  project    = var.project_id

  schema = jsonencode([
    {
      name        = "batch_id"
      type        = "STRING"
      mode        = "REQUIRED"
      description = "Unique batch identifier"
    },
    {
      name        = "product_id"
      type        = "STRING"
      mode        = "REQUIRED"
      description = "Product code"
    },
    {
      name        = "status"
      type        = "STRING"
      mode        = "REQUIRED"
      description = "Batch status (QUEUED, IN_PROGRESS, COMPLETED)"
    },
    {
      name        = "equipment_id"
      type        = "STRING"
      mode        = "REQUIRED"
      description = "Equipment identifier"
    },
    {
      name        = "batch_size"
      type        = "FLOAT64"
      mode        = "REQUIRED"
      description = "Batch size in kg"
    },
    {
      name        = "start_time"
      type        = "TIMESTAMP"
      mode        = "REQUIRED"
      description = "Batch start time"
    },
    {
      name        = "end_time"
      type        = "TIMESTAMP"
      mode        = "NULLABLE"
      description = "Batch end time"
    },
    {
      name        = "loaded_at"
      type        = "TIMESTAMP"
      mode        = "REQUIRED"
      description = "When record was loaded"
    }
  ])

  time_partitioning {
    type  = "DAY"
    field = "start_time"
  }

  clustering = ["equipment_id", "batch_id"]

  labels = {
    environment = var.environment
  }
}

# Additional tables similar to raw_batch_production:
# - raw_sensor_data
# - raw_lab_results
```

**File:** `infrastructure/terraform/environments/dev/main.tf`

**Purpose:** Dev environment configuration

**Content:**

```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }

  # Use local backend for dev (optional)
  # For production, use remote backend (GCS, Terraform Cloud)
}

provider "google" {
  project = var.gcp_project_id
  region  = var.gcp_region
}

# GCS Module
module "gcs" {
  source        = "../modules/gcs"
  project_id    = var.gcp_project_id
  region        = var.gcp_region
  bucket_name   = var.gcs_bucket_name
  environment   = "dev"
}

# BigQuery Module
module "bigquery" {
  source                  = "../modules/bigquery"
  project_id              = var.gcp_project_id
  dataset_id              = var.bigquery_dataset_id
  location                = var.gcp_region
  service_account_email   = var.service_account_email
  environment             = "dev"
}

# Outputs
output "gcs_bucket" {
  value       = module.gcs.bucket_name
  description = "GCS bucket name for data lake"
}

output "bigquery_dataset" {
  value       = module.bigquery.dataset_id
  description = "BigQuery dataset ID"
}
```

**File:** `infrastructure/terraform/environments/dev/terraform.tfvars`

**Purpose:** Terraform variables for dev environment (NOT IN GIT - CREATE MANUALLY)

**Content:**

```hcl
gcp_project_id          = "YOUR_PROJECT_ID"
gcp_region              = "us-central1"
gcs_bucket_name         = "pharma-data-lake-dev-YOUR_PROJECT_ID"
bigquery_dataset_id     = "pharma_analytics_dev"
service_account_email   = "pharma-pipeline@YOUR_PROJECT_ID.iam.gserviceaccount.com"
```

**Deploying Terraform:**

```powershell
# Navigate to dev environment
cd infrastructure/terraform/environments/dev

# Initialize Terraform
terraform init

# Plan changes (dry run)
terraform plan

# Apply changes (create resources)
terraform apply

# Verify resources created in GCP console
# Check: GCS bucket, BigQuery dataset

# Destroy resources (when done learning)
terraform destroy
```

**Deliverables:**
- [ ] Terraform modules created (gcs, bigquery)
- [ ] Dev environment configuration complete
- [ ] Resources successfully deployed to GCP
- [ ] GCS bucket and BigQuery dataset visible in console
- [ ] `terraform.tfvars` in `.gitignore`
- [ ] `.gitignore` updated with `.terraform/`, `*.tfstate*`

---

#### **Day 5-6: Modify Consumer for GCS**

**What You're Doing:**
Update the Kafka consumer to write to Google Cloud Storage instead of local files. This bridges streaming and batch layers.

**File:** `streaming_pipeline/consumers/kafka_to_gcs_consumer.py`

**Key Changes from Local Consumer:**

```python
"""
Kafka to GCS Consumer
Reads messages from Kafka topics and writes them to Google Cloud Storage.
Uses Parquet format for efficient storage.
"""

import logging
import json
from datetime import datetime
from confluent_kafka import Consumer, KafkaError
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroDeserializer
from confluent_kafka.serialization import StringDeserializer
from google.cloud import storage
from google.oauth2 import service_account
import os
from dotenv import load_dotenv

load_dotenv()

# ==================== Configuration ====================
KAFKA_BOOTSTRAP_SERVERS = os.getenv("KAFKA_BOOTSTRAP_SERVERS", "localhost:9092")
SCHEMA_REGISTRY_URL = os.getenv("SCHEMA_REGISTRY_URL", "http://localhost:8081")
GCS_BUCKET_NAME = os.getenv("GCS_BUCKET_NAME")
GCS_PROJECT_ID = os.getenv("GCP_PROJECT_ID")
TOPICS = ["pharma.batch_status", "pharma.sensor_data", "pharma.lab_results"]

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# ==================== GCS Client Setup ====================
def get_gcs_client():
    """Initialize Google Cloud Storage client"""
    credentials = service_account.Credentials.from_service_account_file(
        "./gcp-creds.json"
    )
    return storage.Client(project=GCS_PROJECT_ID, credentials=credentials)

# ==================== Kafka Consumer Setup ====================
def create_kafka_consumer():
    """Create Kafka consumer with Avro deserialization"""
    sr_client = SchemaRegistryClient({"url": SCHEMA_REGISTRY_URL})
    avro_deserializer = AvroDeserializer(sr_client)
    
    consumer_config = {
        "bootstrap.servers": KAFKA_BOOTSTRAP_SERVERS,
        "group.id": "pharma-gcs-consumer",
        "key.deserializer": StringDeserializer("utf_8"),
        "value.deserializer": avro_deserializer,
        "auto.offset.reset": "earliest",
        "enable.auto.commit": True,
        "auto.commit.interval.ms": 5000,
    }
    
    return Consumer(consumer_config)

# ==================== GCS File I/O ====================
def get_gcs_path(topic: str, batch_id: str, timestamp: datetime) -> str:
    """
    Generate GCS path.
    Pattern: gs://bucket/raw/topic_name/batch_id=BATCH-001/date=2025-11-10/hour=14
    Partitioned by: topic, batch_id, date, hour
    """
    topic_name = topic.split(".")[-1]
    date_str = timestamp.strftime("%Y-%m-%d")
    hour_str = timestamp.strftime("%H")
    
    path = f"raw/{topic_name}/batch_id={batch_id}/date={date_str}/hour={hour_str}/"
    return path

def write_message_to_gcs(gcs_client, topic: str, batch_id: str, message: dict):
    """Write message as JSONL to GCS"""
    try:
        timestamp = datetime.utcnow()
        path = get_gcs_path(topic, batch_id, timestamp)
        filename = f"{topic}-{timestamp.isoformat()}.jsonl"
        blob_path = f"{path}{filename}"
        
        bucket = gcs_client.bucket(GCS_BUCKET_NAME)
        blob = bucket.blob(blob_path)
        
        # Write as JSONL
        blob.upload_from_string(
            json.dumps(message) + "\n",
            content_type="application/x-ndjson"
        )
        
        logger.info(f"Wrote message to gs://{GCS_BUCKET_NAME}/{blob_path}")
        
    except Exception as e:
        logger.error(f"Error writing to GCS: {e}")
        raise

# ==================== Main Consumer Logic ====================
def run_consumer():
    """Main consumer loop"""
    consumer = create_kafka_consumer()
    gcs_client = get_gcs_client()
    
    consumer.subscribe(TOPICS)
    
    logger.info(f"Starting Kafka to GCS Consumer")
    logger.info(f"GCS Bucket: {GCS_BUCKET_NAME}")
    logger.info(f"Topics: {TOPICS}")
    
    try:
        while True:
            msg = consumer.poll(timeout=1.0)
            
            if msg is None:
                continue
            
            if msg.error():
                if msg.error().code() == KafkaError._PARTITION_EOF:
                    continue
                else:
                    logger.error(f"Consumer error: {msg.error()}")
                    break
            
            try:
                topic = msg.topic()
                value = msg.value()
                
                # Extract batch_id for partitioning
                batch_id = value.get("batch_id", "unknown")
                
                # Add metadata
                message_with_metadata = {
                    "kafka_topic": topic,
                    "kafka_partition": msg.partition(),
                    "kafka_offset": msg.offset(),
                    "consumed_at": datetime.utcnow().isoformat(),
                    **value
                }
                
                # Write to GCS
                write_message_to_gcs(gcs_client, topic, batch_id, message_with_metadata)
                
            except Exception as e:
                logger.error(f"Error processing message: {e}")
    
    except KeyboardInterrupt:
        logger.info("Consumer interrupted")
    
    finally:
        consumer.close()
        logger.info("Consumer closed")

if __name__ == "__main__":
    run_consumer()
```

**Dependencies to Add to `requirements.txt`:**

```
google-cloud-storage==2.10.0    # GCS access
google-auth==2.25.2              # GCP authentication
```

**Testing:**

```powershell
# Start producers and new GCS consumer
python streaming_pipeline/producers/batch_status_producer.py
python streaming_pipeline/consumers/kafka_to_gcs_consumer.py

# Verify data in GCS (in another terminal)
gcloud storage ls gs://pharma-data-lake-dev-YOUR_PROJECT_ID/raw/
gcloud storage cat gs://pharma-data-lake-dev-YOUR_PROJECT_ID/raw/batch_status/batch_id=BATCH-001/...
```

**Deliverables:**
- [ ] `kafka_to_gcs_consumer.py` created
- [ ] `google-cloud-storage` installed
- [ ] Consumer successfully writing to GCS
- [ ] Data partitioned correctly in GCS
- [ ] `gcp-creds.json` properly secured in `.gitignore`

---

#### **Day 7: Verify End-to-End Cloud Flow**

**What You're Doing:**
Test the complete flow: Producer → Kafka → GCS Consumer → GCS

**Checklist:**

```powershell
# 1. Verify GCS bucket exists
gcloud storage buckets list

# 2. Verify data is in GCS
gcloud storage ls --recursive gs://pharma-data-lake-dev-YOUR_PROJECT_ID/raw/

# 3. View a sample message
gcloud storage cat "gs://pharma-data-lake-dev-YOUR_PROJECT_ID/raw/batch_status/batch_id=BATCH-*/date=*/hour=*/batch_status-*.jsonl" | head -1

# 4. Count messages in GCS
gcloud storage du --summarize gs://pharma-data-lake-dev-YOUR_PROJECT_ID/raw/

# 5. Test Terraform destroy and recreate
cd infrastructure/terraform/environments/dev
terraform destroy
terraform apply
```

**Deliverables:**
- [ ] Phase 3 complete
- [ ] GCS bucket storing data from Kafka
- [ ] Terraform infrastructure version controlled
- [ ] All code committed to Git

---

### **Phase 4: Batch Processing with Spark (Week 4)**
**Goal:** Process raw data from GCS using Spark, clean/enrich it, load to BigQuery.

---

#### **Day 1-2: Spark Environment Setup**

**What You're Doing:**
Set up Apache Spark in Docker to process large datasets. Spark can process data 100x faster than Pandas for large files.

**File:** `docker/spark/Dockerfile`

**Content:**

```dockerfile
FROM bitnami/spark:3.5.0

# Install Python dependencies
RUN pip install --no-cache-dir \
    google-cloud-storage==2.10.0 \
    google-cloud-bigquery==3.13.0 \
    pandas==2.1.3 \
    pyarrow==13.0.0 \
    pyspark==3.5.0

# Copy GCS connector
RUN curl -o /opt/spark/jars/gcs-connector-hadoop3-latest.jar \
    https://storage.googleapis.com/hadoop-releases/gcs/v2.2.11/gcs-connector-hadoop3-2.2.11-shaded.jar
```

**Update `docker-compose.yml`:**

Add Spark service after kafka-ui:

```yaml
  # ======================
  # Apache Spark
  # ======================
  spark-master:
    image: bitnami/spark:3.5.0
    container_name: spark-master
    environment:
      SPARK_MODE: master
      SPARK_RPC_AUTHENTICATION_ENABLED: "no"
      SPARK_RPC_ENCRYPTION_ENABLED: "no"
      SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED: "no"
      SPARK_SSL_ENABLED: "no"
    ports:
      - "8088:8080"  # Spark Master UI
      - "7077:7077"  # Spark Master Port
    volumes:
      - ./batch_pipeline/spark_jobs:/opt/spark/jobs
      - ./gcp-creds.json:/opt/spark/gcp-creds.json
    networks:
      - pharma_network
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"

  spark-worker:
    image: bitnami/spark:3.5.0
    container_name: spark-worker
    depends_on:
      - spark-master
    environment:
      SPARK_MODE: worker
      SPARK_MASTER_URL: spark://spark-master:7077
      SPARK_WORKER_MEMORY: 2G
      SPARK_WORKER_CORES: 2
    ports:
      - "8081:8081"  # Spark Worker UI
    volumes:
      - ./batch_pipeline/spark_jobs:/opt/spark/jobs
      - ./gcp-creds.json:/opt/spark/gcp-creds.json
    networks:
      - pharma_network
```

**Testing Spark:**

```powershell
# Start Docker services
docker-compose up -d

# Test Spark
docker exec spark-master spark-submit --version

# Access Spark UI
# Open browser: http://localhost:8088
```

**Deliverables:**
- [ ] Spark services in docker-compose.yml
- [ ] Spark Master UI accessible
- [ ] GCS credentials mounted

---

#### **Day 3-4: Data Cleansing Job**

**What You're Doing:**
Write a Spark job that reads raw JSON from GCS, cleans it, and writes Parquet files back to GCS.

**File:** `batch_pipeline/spark_jobs/data_cleansing.py`

**Content:**

```python
"""
Data Cleansing Job
Reads raw data from GCS, applies cleansing logic, writes to Parquet.
"""

import sys
from pyspark.sql import SparkSession
from pyspark.sql.functions import (
    col, to_timestamp, when, coalesce, count, 
    isnan, isnull
)
from datetime import datetime
import os

# ==================== Spark Session ====================
def create_spark_session(app_name: str):
    """Create Spark session with GCS credentials"""
    
    spark = SparkSession.builder \
        .appName(app_name) \
        .config("spark.master", "spark://spark-master:7077") \
        .config("spark.executor.memory", "2g") \
        .config("spark.executor.cores", "2") \
        .config("spark.sql.shuffle.partitions", "200") \
        .config("spark.jars", "/opt/spark/jars/gcs-connector-hadoop3-latest.jar") \
        .getOrCreate()
    
    # Set GCS credentials
    hadoop_conf = spark.sparkContext._jsc.hadoopConfiguration()
    hadoop_conf.set("google.cloud.auth.service.account.json.keyfile", "/opt/spark/gcp-creds.json")
    
    return spark

# ==================== Data Cleansing Functions ====================
def cleanse_batch_data(df):
    """
    Cleanse batch production data.
    - Remove duplicates
    - Standardize timestamps
    - Fill missing values
    """
    
    print("Cleansing batch production data...")
    
    # Remove exact duplicates
    df = df.dropDuplicates()
    
    # Standardize timestamp columns to UTC
    df = df.withColumn("start_time", to_timestamp(col("start_time")))
    df = df.withColumn("end_time", to_timestamp(col("end_time")))
    df = df.withColumn("event_timestamp", to_timestamp(col("event_timestamp")))
    
    # Fill missing end_time with current timestamp for in-progress batches
    df = df.withColumn(
        "end_time",
        when(col("status") == "IN_PROGRESS", None)
        .otherwise(col("end_time"))
    )
    
    # Add metadata columns
    df = df.withColumn("_load_date", to_timestamp(col("_load_date")))
    df = df.withColumn("_processed_at", to_timestamp(datetime.utcnow()))
    
    print(f"Cleansed {df.count()} records")
    return df

def cleanse_sensor_data(df):
    """Cleanse sensor readings"""
    
    print("Cleansing sensor data...")
    
    # Remove duplicates based on sensor_id, batch_id, reading_timestamp
    df = df.dropDuplicates(["sensor_id", "batch_id", "reading_timestamp"])
    
    # Standardize timestamps
    df = df.withColumn("reading_timestamp", to_timestamp(col("reading_timestamp")))
    
    # Handle null sensor values
    df = df.where(col("value").isNotNull())
    
    # Validate specification bounds
    df = df.withColumn(
        "is_out_of_spec",
        (col("value") < col("specification_min")) | 
        (col("value") > col("specification_max"))
    )
    
    print(f"Cleansed {df.count()} sensor records")
    return df

def cleanse_lab_results(df):
    """Cleanse lab test results"""
    
    print("Cleansing lab results...")
    
    # Remove duplicates
    df = df.dropDuplicates()
    
    # Standardize timestamp
    df = df.withColumn("test_timestamp", to_timestamp(col("test_timestamp")))
    
    # Validate pass_fail enum
    valid_results = ["PASS", "FAIL", "CONDITIONAL"]
    df = df.where(col("pass_fail").isin(valid_results))
    
    # Validate test result bounds
    df = df.withColumn(
        "is_within_spec",
        (col("test_result") >= col("specification_min")) & 
        (col("test_result") <= col("specification_max"))
    )
    
    print(f"Cleansed {df.count()} lab records")
    return df

# ==================== Quality Checks ====================
def data_quality_report(df, table_name: str):
    """Generate data quality report"""
    
    print(f"\n=== Data Quality Report: {table_name} ===")
    print(f"Total records: {df.count()}")
    
    # Check for nulls
    null_counts = df.select([
        count(when(isnull(col(c)), 1)).alias(c)
        for c in df.columns
    ])
    print(f"Null value counts:\n{null_counts.show(truncate=False)}")
    
    print("==========================================\n")

# ==================== Main Job ====================
def main():
    """Main Spark job"""
    
    spark = create_spark_session("PharmaBatchCleansing")
    
    # GCS paths
    gcs_bucket = "pharma-data-lake-dev"  # From terraform
    raw_path = f"gs://{gcs_bucket}/raw"
    processed_path = f"gs://{gcs_bucket}/processed"
    
    try:
        # ===== Cleanse Batch Data =====
        batch_df = spark.read \
            .option("multiline", "true") \
            .json(f"{raw_path}/batch_status/")
        
        batch_df = cleanse_batch_data(batch_df)
        data_quality_report(batch_df, "batch_production")
        
        # Write to Parquet
        batch_df.write \
            .mode("overwrite") \
            .parquet(f"{processed_path}/batch_production/")
        print(f"Wrote batch data to {processed_path}/batch_production/")
        
        # ===== Cleanse Sensor Data =====
        sensor_df = spark.read \
            .option("multiline", "true") \
            .json(f"{raw_path}/sensor_data/")
        
        sensor_df = cleanse_sensor_data(sensor_df)
        data_quality_report(sensor_df, "sensor_data")
        
        sensor_df.write \
            .mode("overwrite") \
            .parquet(f"{processed_path}/sensor_data/")
        print(f"Wrote sensor data to {processed_path}/sensor_data/")
        
        # ===== Cleanse Lab Results =====
        lab_df = spark.read \
            .option("multiline", "true") \
            .json(f"{raw_path}/lab_results/")
        
        lab_df = cleanse_lab_results(lab_df)
        data_quality_report(lab_df, "lab_results")
        
        lab_df.write \
            .mode("overwrite") \
            .parquet(f"{processed_path}/lab_results/")
        print(f"Wrote lab data to {processed_path}/lab_results/")
        
        print("\n✅ Data cleansing job completed successfully!")
        
    except Exception as e:
        print(f"❌ Job failed: {e}")
        raise
    
    finally:
        spark.stop()

if __name__ == "__main__":
    main()
```

**Deliverables:**
- [ ] `data_cleansing.py` created
- [ ] Job runs successfully on sample data
- [ ] Parquet files written to GCS

---

#### **Day 5-6: Feature Engineering**

**File:** `batch_pipeline/spark_jobs/feature_engineering.py`

**Content (abbreviated - key sections):**

```python
"""
Feature Engineering Job
Enriches cleaned data with computed features for analytics.
- Batch yield calculation
- Temperature statistics
- Quality metrics aggregation
"""

from pyspark.sql import SparkSession
from pyspark.sql.functions import (
    col, avg, min, max, count, sum as spark_sum,
    when, round, window
)
from pyspark.sql.window import Window

def create_spark_session(app_name: str):
    spark = SparkSession.builder \
        .appName(app_name) \
        .config("spark.master", "spark://spark-master:7077") \
        .getOrCreate()
    return spark

def engineer_batch_features(batch_df, sensor_df, lab_df):
    """
    Engineer features for batch analytics.
    Joins batch, sensor, and lab data.
    """
    
    # Join batch with sensor data to get temperature stats
    batch_with_sensors = batch_df.join(
        sensor_df.filter(col("sensor_type") == "TEMPERATURE"),
        on="batch_id",
        how="left"
    )
    
    # Calculate average temperature per batch
    temp_stats = batch_with_sensors.groupBy("batch_id") \
        .agg(
            avg(col("value")).alias("avg_temperature"),
            min(col("value")).alias("min_temperature"),
            max(col("value")).alias("max_temperature"),
            count(when(col("is_out_of_spec"), 1)).alias("temp_violations")
        )
    
    # Join with lab results for quality metrics
    batch_with_quality = batch_df.join(
        lab_df,
        on="batch_id",
        how="left"
    )
    
    # Calculate batch pass rate
    quality_stats = batch_with_quality.groupBy("batch_id") \
        .agg(
            count(col("test_id")).alias("total_tests"),
            count(when(col("pass_fail") == "PASS", 1)).alias("passed_tests"),
            (count(when(col("pass_fail") == "PASS", 1)) / 
             count(col("test_id")) * 100).alias("pass_rate_pct")
        )
    
    # Calculate yield: (actual output / expected output) * 100
    # For demo: assume 10% loss is normal, flag if > 15%
    features = batch_df.join(temp_stats, on="batch_id", how="left") \
        .join(quality_stats, on="batch_id", how="left")
    
    features = features.withColumn(
        "estimated_yield_pct",
        when(col("status") == "COMPLETED", 
             (col("batch_size") * 0.85) / col("batch_size") * 100)
        .otherwise(None)
    )
    
    features = features.withColumn(
        "quality_flag",
        when(col("pass_rate_pct") < 90, "LOW_QUALITY")
        .when(col("temp_violations") > 5, "TEMP_OUT_OF_SPEC")
        .otherwise("OK")
    )
    
    return features

def main():
    spark = create_spark_session("PharmaFeatureEngineering")
    
    # Read processed data
    batch_df = spark.read.parquet("gs://pharma-data-lake-dev/processed/batch_production/")
    sensor_df = spark.read.parquet("gs://pharma-data-lake-dev/processed/sensor_data/")
    lab_df = spark.read.parquet("gs://pharma-data-lake-dev/processed/lab_results/")
    
    # Engineer features
    enriched_df = engineer_batch_features(batch_df, sensor_df, lab_df)
    
    # Write to BigQuery-ready format
    enriched_df.write \
        .mode("overwrite") \
        .parquet("gs://pharma-data-lake-dev/enriched/batch_with_features/")
    
    print("✅ Feature engineering complete!")
    spark.stop()

if __name__ == "__main__":
    main()
```

**Deliverables:**
- [ ] `feature_engineering.py` created and tested
- [ ] Enriched Parquet files created in GCS

---

#### **Day 7: Load to BigQuery**

**Update Terraform:** Add BigQuery table creation

**File:** `infrastructure/terraform/modules/bigquery/main.tf` (add to end)

```hcl
# External table pointing to processed Parquet
resource "google_bigquery_table" "fact_batch_production" {
  dataset_id = google_bigquery_dataset.pharma_analytics.dataset_id
  table_id   = "fact_batch_production"
  project    = var.project_id

  external_data_configuration {
    autodetect    = true
    source_format = "PARQUET"
    source_uris   = ["gs://${var.gcs_bucket_name}/enriched/batch_with_features/*"]
  }

  clustering = ["batch_id", "equipment_id"]

  labels = {
    environment = var.environment
  }
}
```

**Test BigQuery:**

```powershell
# Run Spark jobs
docker exec spark-master spark-submit /opt/spark/jobs/data_cleansing.py
docker exec spark-master spark-submit /opt/spark/jobs/feature_engineering.py

# Query BigQuery
bq query "SELECT COUNT(*) as total_batches, AVG(pass_rate_pct) FROM pharma_analytics.fact_batch_production"
```

**Deliverables:**
- [ ] Phase 4 complete
- [ ] Spark jobs running successfully
- [ ] Data flowing: GCS → Spark → Parquet → BigQuery
- [ ] Ability to query BigQuery for batch analytics

---

### **Phase 5: dbt Transformations (Week 5)**
**Goal:** Transform raw data into business-ready analytics models using dbt.

---

*(Abbreviated due to length - follows similar pattern to Phase 4)*

#### **Key Components:**

**dbt Project Structure:**
```
transformations/dbt/
├── models/
│   ├── staging/
│   │   ├── stg_batch_production.sql
│   │   ├── stg_sensor_data.sql
│   │   └── stg_lab_results.sql
│   ├── intermediate/
│   │   └── int_batch_enriched.sql
│   └── marts/
│       ├── fact_batch_production.sql
│       └── dim_equipment.sql
├── tests/
├── macros/
└── dbt_project.yml
```

**Setup:**

```powershell
cd transformations/dbt
dbt init
# Configure profiles.yml with BigQuery connection

dbt debug  # Test connection
dbt run    # Run all models
dbt test   # Run data quality tests
```

**Learning Focus:**
- Staging layer: Typecast and clean raw data
- Intermediate: Apply business logic and joins
- Marts: Final analytics-ready tables
- Tests: Ensure data quality (unique, not null, relationships)
- Incremental models: Process only new data

**Deliverables:**
- [ ] dbt project initialized
- [ ] 3 staging models working
- [ ] 2 mart models with incremental logic
- [ ] 5+ data quality tests passing

---

### **Phase 6: Orchestration with Airflow (Week 6)**
**Goal:** Automate entire pipeline execution with Apache Airflow.

---

**Key DAGs:**

1. **streaming_to_gcs_dag.py** - Continuously run Kafka consumer
2. **batch_processing_dag.py** - Daily: Cleansing → Feature Engineering → BigQuery Load
3. **dbt_transformation_dag.py** - Daily (after batch): dbt models → tests

**Setup:**

```powershell
# Add to docker-compose.yml
# Create airflow/Dockerfile
# Initialize Airflow database

docker-compose up airflow-webserver

# Access: http://localhost:8080
# Username: airflow / Password: airflow
```

**Deliverables:**
- [ ] Airflow running in Docker
- [ ] 3 DAGs deployed and scheduled
- [ ] Task dependencies configured
- [ ] Monitoring and alerting active

---

### **Phase 7: Visualization with Metabase (Week 7)**
**Goal:** Create interactive dashboards for business insights.

---

**Dashboard Tiles:**

1. **Batch Quality Overview**
   - Pass rate trend
   - Total batches processed
   - Recent failures

2. **Production Performance**
   - Equipment utilization
   - Yield by equipment
   - Temperature trends

3. **Compliance Metrics**
   - Open deviations
   - Investigation time
   - Quality trends

**Setup:**

```powershell
# Add to docker-compose.yml
docker-compose up metabase

# Access: http://localhost:3000
# Connect to BigQuery
# Create dashboards from SQL queries
```

**Deliverables:**
- [ ] Metabase connected to BigQuery
- [ ] 3-tile dashboard created
- [ ] Filters and drill-downs working
- [ ] Shared with team

---

### **Phase 8: Documentation & Portfolio Polish (Week 8)**
**Goal:** Make project reproducible and portfolio-ready.

---

**Checklist:**

1. **README.md**
   - Project overview
   - Architecture diagram
   - Setup instructions (< 30 minutes)
   - Troubleshooting guide

2. **Data Dictionary**
   - All table schemas
   - Column descriptions
   - Data lineage

3. **Code Quality**
   - Unit tests
   - Integration tests
   - Linting and formatting

4. **Demo**
   - 5-10 minute video
   - Talking points
   - Portfolio-ready presentation

**Deliverables:**
- [ ] README with complete setup guide
- [ ] Architecture diagram
- [ ] Data dictionary
- [ ] 70%+ test coverage
- [ ] Demo video
- [ ] All code committed to Git (100+ commits)

---

## **🚀 Getting Started Today**

**Start with this checklist:**
1. [ ] Read this entire document
2. [ ] Set aside dedicated time each week (10-15 hours)
3. [ ] Install Docker Desktop
4. [ ] Create a GitHub repository
5. [ ] Clone the repo locally
6. [ ] Start Phase 1, Day 1

**Pro Tips:**
- Don't skip weeks - build momentum
- Commit code daily (even work-in-progress)
- Document your learnings in a journal
- Join data engineering communities (Reddit, Discord)
- Ask questions when stuck (don't waste hours)

---

## **📊 Project Evaluation Criteria**

Your project will meet production-level standards by addressing these criteria:

### **Problem Description (4/4 points)**

The pharmaceutical manufacturing industry operates under stringent regulatory requirements (FDA, GMP) and faces significant challenges in managing vast amounts of data generated during production. Key problems include:

*   **Data Silos:** Critical data from Manufacturing Execution Systems (MES), Laboratory Information Management Systems (LIMS), and equipment sensors are isolated. This fragmentation prevents a holistic view of the batch lifecycle.
*   **Reactive Quality Control:** Batch quality is often assessed post-production. Delays in identifying deviations lead to increased waste, costly investigations, and potential product recalls.
*   **Compliance Burden:** Manually compiling data for regulatory audits is time-consuming, error-prone, and makes it difficult to prove data integrity as required by regulations like 21 CFR Part 11.
*   **Operational Inefficiency:** Without real-time insights, it's difficult to correlate process parameters with quality outcomes, optimize equipment usage, or predict batch failures.

This project will solve these problems by creating a unified, end-to-end data platform. It will ingest, process, and analyze manufacturing data in real-time to provide actionable insights for quality assurance, operational efficiency, and streamlined regulatory compliance.

### **Cloud (4/4 points)**

The entire project will be developed and deployed on **Google Cloud Platform (GCP)**. Infrastructure will be managed using **Terraform**, an Infrastructure as Code (IaC) tool. This ensures the environment is version-controlled, repeatable, and can be easily provisioned or torn down.

*   **GCP Services:**
    *   **Google Compute Engine (GCE):** To host services like Kafka, Spark, and Airflow within Docker containers.
    *   **Google Cloud Storage (GCS):** To serve as the data lake for storing raw and processed data.
    *   **BigQuery:** To act as the scalable, serverless data warehouse.
*   **IaC:**
    *   **Terraform:** To define and manage all cloud resources, including GCS buckets, BigQuery datasets, and service account permissions.

### **Data Ingestion (4/4 points)**

A streaming architecture will be implemented using **Apache Kafka** to handle real-time data ingestion.

*   **Producers:** Custom Python scripts will simulate data from various manufacturing sources (e.g., MES, equipment sensors, LIMS) and publish messages to different Kafka topics.
*   **Consumers:** Kafka consumers will process these streams in real-time.
*   **Streaming Technologies:** An Airflow DAG will orchestrate a streaming pipeline where a consumer reads from Kafka topics and lands the raw data into the GCS data lake, making it available for batch processing.
*   **Schema Management:** Confluent Schema Registry will be used to enforce data schemas and ensure data quality from the point of ingestion.

### **Data Warehouse (4/4 points)**

**Google BigQuery** will be used as the data warehouse. The table structures will be optimized for analytical performance.

*   **Tables:** We will create fact and dimension tables to model the manufacturing process (e.g., `fact_batch_production`, `dim_equipment`).
*   **Partitioning:** Fact tables will be partitioned by a `DATE` or `TIMESTAMP` column (e.g., `production_date`). This will significantly speed up queries that filter by time ranges, which is a common use case for analyzing batch data.
*   **Clustering:** Tables will be clustered by frequently used filter or join keys like `batch_id` and `equipment_id`. This will co-locate related data, reducing the amount of data scanned and improving query performance. An explanation for these choices will be included in the dbt documentation.

### **Transformations (4/4 points)**

Data transformations will be handled using a combination of **Spark** and **dbt (data build tool)**.

*   **Spark:** Used for large-scale batch processing of raw data from the data lake. Transformations will include data cleaning, normalization, and feature engineering.
*   **dbt:** Used for in-warehouse transformations on BigQuery. It will define the business logic to create analytics-ready models, enforce data quality tests, and automatically generate data lineage documentation.

### **Dashboard (4/4 points)**

An interactive dashboard will be created using **Metabase** to visualize the key performance indicators (KPIs) and insights from the data warehouse.

*   **Tile 1: Batch Quality Overview:** This tile will display real-time metrics such as the current batch pass/fail rate, average batch yield, and the number of active deviations.
*   **Tile 2: Production Performance Analysis:** This tile will include visualizations for equipment utilization, trend analysis of process parameters (e.g., temperature over time for a specific batch), and correlations between material lots and quality outcomes.
*   **Tile 3: Compliance Dashboard:** This tile will show metrics related to regulatory compliance, such as the average time to close an investigation and the number of open CAPAs (Corrective and Preventive Actions).

### **Reproducibility (4/4 points)**

The project will be designed for easy reproducibility.

*   **Clear Instructions:** A `README.md` file will provide comprehensive, step-by-step instructions for setting up the environment, deploying the infrastructure, and running the entire pipeline.
*   **Automation Scripts:** Shell scripts (e.g., `build.sh`) will be provided to automate the setup process, including building Docker images and starting all services.
*   **Docker Compose:** All services (Kafka, Airflow, Spark, etc.) will be containerized using Docker and managed with a `docker-compose.yml` file, ensuring a consistent and isolated environment.
*   **Environment Configuration:** An `.env.example` file will be provided to make configuration of environment variables straightforward.

---

## **📁 Project Directory Structure**

This section will guide you through creating a production-level data engineering project structure. Understanding **why** each directory exists is crucial for real-world projects.

### **Recommended Directory Structure**

```
Sensorlytics/
│
├── streaming_pipeline/          # Real-time data ingestion layer
│   ├── producers/               # Kafka producers (data generators)
│   ├── consumers/               # Kafka consumers (data processors)
│   └── schemas/                 # Avro/JSON schemas for data validation
│
├── batch_pipeline/              # Batch processing layer
│   ├── spark_jobs/              # PySpark transformation scripts
│   ├── processed_data/          # Local output (development only)
│   └── jars/                    # Spark dependencies (connectors, etc.)
│
├── transformations/             # Data transformation layer
│   └── dbt/                     # dbt project for SQL transformations
│       ├── models/              # SQL models (staging, marts)
│       ├── tests/               # Data quality tests
│       ├── macros/              # Reusable SQL functions
│       └── dbt_project.yml      # dbt configuration
│
├── orchestration/               # Workflow orchestration
│   └── airflow/                 # Airflow DAGs and plugins
│       ├── dags/                # DAG definitions
│       ├── plugins/             # Custom operators/sensors
│       └── config/              # Airflow configuration
│
├── infrastructure/              # Infrastructure as Code
│   └── terraform/               # Terraform configurations
│       ├── modules/             # Reusable Terraform modules
│       ├── environments/        # Dev/Prod configurations
│       └── variables.tf         # Variable definitions
│
├── docker/                      # Docker configurations
│   ├── kafka/                   # Kafka Docker setup
│   ├── spark/                   # Spark Docker setup
│   ├── airflow/                 # Airflow Docker setup
│   └── metabase/                # Metabase Docker setup
│
├── dashboards/                  # Dashboard and visualization
│   └── metabase/                # Metabase dashboard exports
│
├── scripts/                     # Utility scripts
│   ├── setup/                   # Setup and initialization scripts
│   ├── data_quality/            # Data validation scripts
│   └── monitoring/              # Monitoring and alerting scripts
│
├── tests/                       # Testing
│   ├── unit/                    # Unit tests
│   ├── integration/             # Integration tests
│   └── fixtures/                # Test data
│
├── docs/                        # Documentation
│   ├── architecture/            # Architecture diagrams
│   ├── data_dictionary/         # Data catalog/dictionary
│   └── runbooks/                # Operational guides
│
├── logs/                        # Application logs (gitignored)
│
├── .env.example                 # Example environment variables
├── .gitignore                   # Git ignore patterns
├── docker-compose.yml           # Docker orchestration
├── requirements.txt             # Python dependencies
├── README.md                    # Project documentation
├── plan.md                      # This file
└── LICENSE                      # Project license
```

### **Directory-by-Directory Reference**

> **Note:** You don't need to create all these folders upfront. Create them as you progress through the phases.

#### **1. `streaming_pipeline/` - Why This Matters**

**Purpose:** Handles real-time data ingestion. In pharma manufacturing, sensor data (temperature, humidity) and batch status changes happen continuously.

**What You'll Learn:**
- How to design Kafka topics (naming conventions, partitioning strategy)
- Schema evolution and backward compatibility
- Error handling in streaming systems
- Exactly-once vs at-least-once semantics

**Directory Breakdown:**
```
streaming_pipeline/
├── producers/
│   ├── batch_status_producer.py      # Simulates MES batch events
│   ├── sensor_data_producer.py       # Simulates equipment sensors
│   └── lab_results_producer.py       # Simulates LIMS test results
├── consumers/
│   ├── kafka_to_gcs_consumer.py      # Lands raw data to GCS
│   └── consumer_config.py            # Shared consumer configuration
└── schemas/
    ├── batch_status.avsc             # Avro schema for batch events
    └── sensor_data.avsc              # Avro schema for sensor readings
```

**Key Learning Questions:**
- How many partitions should your Kafka topics have?
- Should you use Avro or JSON schemas? (Hint: Avro is more efficient)
- How do you handle late-arriving data?

---

#### **2. `batch_pipeline/` - Why This Matters**

**Purpose:** Processes large volumes of historical data. Spark is used for complex transformations that don't fit in SQL.

**What You'll Learn:**
- PySpark dataframe operations
- Optimizing Spark jobs (partitioning, caching)
- Writing data to cloud storage (Parquet format)
- Data quality checks at scale

**Directory Breakdown:**
```
batch_pipeline/
├── spark_jobs/
│   ├── data_cleansing.py             # Clean and normalize raw data
│   ├── feature_engineering.py        # Calculate batch metrics
│   └── export_to_bigquery.py         # Load processed data to BigQuery
├── processed_data/                    # Local development output
└── jars/
    └── gcs-connector-hadoop3.jar     # GCS connector for Spark
```

**Key Learning Questions:**
- When should you use Spark vs SQL transformations?
- How do you partition Parquet files for optimal query performance?
- What's the difference between `coalesce()` and `repartition()`?

---

#### **3. `transformations/dbt/` - Why This Matters**

**Purpose:** Business logic lives here. dbt transforms raw data into analytics-ready models using SQL.

**What You'll Learn:**
- Dimensional modeling (star schema)
- Incremental models vs full refresh
- Data testing (not null, unique, referential integrity)
- DAG dependencies in SQL

**Directory Breakdown:**
```
transformations/dbt/
├── models/
│   ├── staging/                       # Clean, typed raw data
│   │   ├── stg_batch_production.sql
│   │   └── stg_quality_tests.sql
│   ├── intermediate/                  # Business logic transformations
│   │   └── int_batch_enriched.sql
│   └── marts/                         # Final analytics tables
│       ├── fact_batch_production.sql
│       └── dim_equipment.sql
├── tests/
│   └── assert_batch_yield_valid.sql   # Custom data tests
├── macros/
│   └── generate_batch_id.sql          # Reusable SQL functions
└── dbt_project.yml                    # dbt configuration
```

**Key Learning Questions:**
- What's the difference between staging, intermediate, and marts layers?
- How do you handle slowly changing dimensions (SCD Type 2)?
- When should you use `ref()` vs `source()` in dbt?

---

#### **4. `orchestration/airflow/` - Why This Matters**

**Purpose:** Coordinates the entire pipeline. Airflow ensures tasks run in the correct order and handles retries/failures.

**What You'll Learn:**
- DAG design patterns
- Task dependencies and parallel execution
- Idempotency (running the same DAG multiple times safely)
- Monitoring and alerting

**Directory Breakdown:**
```
orchestration/airflow/
├── dags/
│   ├── streaming_to_gcs_dag.py        # Kafka → GCS pipeline
│   ├── batch_processing_dag.py        # Spark processing pipeline
│   └── dbt_transformation_dag.py      # dbt transformation pipeline
├── plugins/
│   └── custom_operators/              # Custom task operators
└── config/
    └── airflow.cfg                    # Airflow settings
```

**Key Learning Questions:**
- What's the difference between `@daily` and `schedule_interval='0 0 * * *'`?
- How do you pass data between tasks (XCom vs external storage)?
- What's an idempotent DAG and why does it matter?

---

#### **5. `infrastructure/terraform/` - Why This Matters**

**Purpose:** Defines cloud infrastructure as code. Makes it easy to recreate environments.

**What You'll Learn:**
- IaC principles (version control for infrastructure)
- GCP resource provisioning
- Managing state files
- Environment separation (dev vs prod)

**Directory Breakdown:**
```
infrastructure/terraform/
├── modules/
│   ├── gcs/                           # GCS bucket module
│   ├── bigquery/                      # BigQuery dataset module
│   └── iam/                           # Service account module
├── environments/
│   ├── dev/
│   │   └── main.tf                    # Dev environment config
│   └── prod/
│       └── main.tf                    # Prod environment config
├── variables.tf                       # Input variables
└── outputs.tf                         # Output values
```

**Key Learning Questions:**
- What's the difference between Terraform and manual cloud console setup?
- How do you manage sensitive data (service account keys)?
- What's a Terraform state file and why is it important?

---

#### **6. `docker/` - Why This Matters**

**Purpose:** Containerizes all services for consistent local development.

**What You'll Learn:**
- Docker fundamentals
- Multi-service orchestration with Docker Compose
- Volume mounting for development
- Networking between containers

**Directory Breakdown:**
```
docker/
├── kafka/
│   └── Dockerfile                     # Custom Kafka image
├── spark/
│   └── Dockerfile                     # Spark with Python dependencies
├── airflow/
│   └── Dockerfile                     # Airflow with custom packages
└── metabase/
    └── Dockerfile                     # Metabase image
```

**Key Learning Questions:**
- What's the difference between `CMD` and `ENTRYPOINT` in a Dockerfile?
- How do you persist data across container restarts?
- What's a Docker network and why do you need it?

---

### **📝 .env.example Template**

Create this file in your project root:

```bash
# GCP Configuration
GCP_PROJECT_ID=your-project-id
GCP_REGION=us-central1
GCS_BUCKET_NAME=pharma-data-lake
GCS_RAW_DATA_PATH=raw/
GCS_PROCESSED_DATA_PATH=processed/

# BigQuery Configuration
BQ_DATASET_NAME=pharma_analytics
BQ_LOCATION=US

# Kafka Configuration
KAFKA_BOOTSTRAP_SERVERS=localhost:9092
SCHEMA_REGISTRY_URL=http://localhost:8081

# Airflow Configuration
AIRFLOW_UID=50000
AIRFLOW_GID=0
AIRFLOW_PROJ_DIR=./orchestration/airflow

# Metabase Configuration
METABASE_PORT=3000
```

---

## **⚠️ Common Pitfalls to Avoid**

| Pitfall | Why It's Bad | What To Do Instead |
|---------|--------------|-------------------|
| **Starting with everything at once** | Overwhelming, hard to debug | Build incrementally. Get Kafka working before adding Spark |
| **Ignoring data quality early** | Garbage in, garbage out | Add schema validation from day one |
| **Hardcoding credentials** | Security risk, not portable | Use environment variables from the start |
| **Not testing locally first** | Wastes cloud credits, slow feedback | Use Docker Compose to test everything locally |
| **Skipping documentation** | Future you won't remember why | Document as you build, not at the end |
| **Copy-pasting without understanding** | Can't troubleshoot or extend | Type out code, understand each line |
| **Perfectionism** | Never ship anything | Done is better than perfect - iterate! |

---

## **📚 Learning Resources**

### **Core Technologies**
- **Kafka:** [Apache Kafka Docs](https://kafka.apache.org/documentation/), [Confluent Tutorials](https://developer.confluent.io/)
- **Spark:** "Learning Spark" by O'Reilly, [Spark by Examples](https://sparkbyexamples.com/)
- **dbt:** [dbt Docs](https://docs.getdbt.com/), [dbt Learn (Free Courses)](https://courses.getdbt.com/)
- **Airflow:** [Apache Airflow Docs](https://airflow.apache.org/), [Astronomer Guides](https://www.astronomer.io/guides/)
- **Terraform:** [HashiCorp Learn](https://learn.hashicorp.com/terraform)
- **Docker:** [Docker Docs](https://docs.docker.com/), Docker Mastery course on Udemy

### **Communities to Join**
- r/dataengineering (Reddit)
- Data Engineering Discord servers
- dbt Slack community
- Local meetups (Kafka, Airflow, etc.)

### **Portfolio Building**
- Document your learnings on Medium or Dev.to
- Share progress on LinkedIn
- Open-source your code on GitHub
- Present at local meetups

---

## **🎓 Learning Mindset: Questions to Ask Yourself**

As you build each component, constantly ask:

1. **Why am I using this technology?**  
   ➜ Don't use Spark just because it's cool. Could SQL do this?

2. **What happens if this fails?**  
   ➜ Design for failure. Add retries, dead letter queues, monitoring.

3. **How do I test this?**  
   ➜ Unit tests for logic, integration tests for components.

4. **Can someone else run this?**  
   ➜ Reproducibility check. Clear README, no hidden steps.

5. **Is this production-ready?**  
   ➜ Logging, monitoring, error handling, documentation.

6. **How would this scale?**  
   ➜ 10x more data? 100x? What breaks first?

7. **What's the business value?**  
   ➜ How does this tile help someone make a decision?

---

## **🎯 Success Metrics**

By the end of 8 weeks, you should have:

- [ ] A GitHub repo with 100+ commits
- [ ] A working end-to-end data pipeline
- [ ] Infrastructure defined as code (Terraform)
- [ ] Automated orchestration (Airflow)
- [ ] Data quality tests passing
- [ ] Interactive dashboards with real insights
- [ ] Comprehensive documentation
- [ ] A 5-10 minute demo video
- [ ] Portfolio piece for your resume
- [ ] Hands-on experience to discuss in interviews

---

## **💡 Remember**

> **"The goal is to learn the engineering process, not just copy code."**

Take your time with each phase. Truly understand the **"why"** behind each decision. This project is your training ground for real-world data engineering.

When you're stuck:
1. Check the logs
2. Google the error
3. Ask in communities
4. Take a break and come back fresh
5. Simplify and start over if needed

**You've got this! Start with Phase 1, Day 1. 🚀**

---

## **📎 Appendix: Supporting Information**
