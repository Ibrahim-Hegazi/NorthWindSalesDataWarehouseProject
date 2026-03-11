# Complete Project Structure & Service Description

## 📁 Your Workspace Directory Structure

```
your-project/
│
├── 📄 docker-compose.yml
├── 📄 .env
│
├── 📁 postgres-airflow/          # Airflow metadata database
│   └── 📁 data/                   # PostgreSQL binary files
│
├── 📁 postgres-app/               # Application database
│   └── 📁 data/                    # PostgreSQL binary files
│
├── 📁 pgadmin/                    # pgAdmin settings
│
├── 📁 airflow/                    # Airflow core files
│   ├── 📁 dags/                     # Your DAG definitions
│   ├── 📁 logs/                      # All Airflow logs
│   ├── 📁 plugins/                   # Custom Airflow plugins
│   └── 📁 scripts/                   # Helper scripts
│
├── 📁 spark_jobs/                 # Shared Spark applications
│
├── 📁 pyspark_code/               # Jupyter notebooks
│
├── 📁 checkpoint/                 # Spark streaming checkpoints
│
├── 📁 data/                       # Shared data files
│
├── 📁 python_code/                # Python applications/APIs
│
└── 📁 generators/                 # Data generator scripts
```

---

## 📊 Complete Service Reference Table

| Service | Service Type | What It Does | Code Location (Host) | Input Directory | Output Directory | Port | Access URL | When to Use |
|---------|--------------|--------------|---------------------|-----------------|------------------|------|------------|-------------|
| **postgres-airflow** | Database | Stores Airflow metadata (DAG runs, task states, connections) | N/A | `./postgres-airflow/data/` (DB files) | Same as input | `5432` | `localhost:5432` | **NEVER write directly** - Airflow owns this DB |
| **postgres-app** | Database | Stores your application/business data | N/A | `./postgres-app/data/` (DB files) | Same as input | `5433` | `localhost:5433` | Store results from Spark jobs, API data, etc. |
| **redis** | Message Broker | Handles communication between Airflow components | N/A | No persistent storage | No persistent storage | `6379` | `localhost:6379` | Internal Airflow communication only |
| **pgadmin** | Management UI | Web interface to manage both PostgreSQL databases | `./pgadmin/` (settings) | `./pgadmin/` | Same as input | `5050` | `http://localhost:5050` | Manual DB inspection, CSV imports, SQL queries |
| **airflow-webserver** | Orchestration | Airflow UI, DAG parser, trigger DAG runs | `./airflow/dags/` | `./airflow/dags/` (DAGs) | `./airflow/logs/` (logs) | `8080` | `http://localhost:8080` | Monitor DAGs, trigger runs, check task status |
| **airflow-scheduler** | Orchestration | Schedules when to run DAGs | Same as webserver | Same as webserver | Same as webserver | None | N/A | Internal - runs continuously |
| **airflow-worker** | Orchestration | Executes individual tasks | Same as webserver | Same as webserver | Same as webserver | None | N/A | Internal - executes your DAG tasks |
| **broker** | Streaming | Kafka message broker for real-time data | N/A | No persistent storage (ephemeral) | No persistent storage | `9092` | `localhost:9092` | Real-time data ingestion/streaming |
| **schema-registry** | Streaming | Manages Kafka message schemas (Avro/Protobuf) | N/A | No persistent storage | No persistent storage | `8081` | `http://localhost:8081` | When using structured messages in Kafka |
| **kafka-ui** | Management UI | Visual interface for Kafka topics/messages | N/A | No persistent storage | No persistent storage | `8090` | `http://localhost:8090` | Monitor Kafka, view messages, manage topics |
| **pyspark-dev** | Processing | Jupyter Lab with PySpark for development | `./pyspark_code/`<br>`./spark_jobs/`<br>`./data/`<br>`./checkpoint/` | `./pyspark_code/` (notebooks)<br>`./spark_jobs/` (scripts)<br>`./data/` (input data)<br>`./checkpoint/` | `./pyspark_code/` (saved)<br>`./spark_jobs/` (output)<br>`./data/` (results)<br>`./checkpoint/` | `8888`<br>`4040` | `http://localhost:8888?token=devtoken`<br>`http://localhost:4040` | Develop Spark jobs, test transformations |
| **python-dev** | Development | General Python environment for apps/APIs | `./python_code/`<br>`./data/` | `./python_code/` (scripts)<br>`./data/` (input) | `./python_code/` (output)<br>`./data/` (output) | None | N/A | Build FastAPI apps, data utilities, scripts |
| **data-generator** | Development | Generates test data to Kafka | `./generators/` | `./generators/` (scripts) | N/A (writes to Kafka) | None | N/A | Create realistic test data streams |

---

## 📂 Detailed Directory Reference

| Directory | Purpose | File Types | Read By | Written By | Backup Priority |
|-----------|---------|------------|---------|------------|-----------------|
| `./postgres-airflow/data/` | Airflow metadata | PostgreSQL binary files | postgres-airflow | postgres-airflow | ⭐⭐⭐ CRITICAL - Airflow state |
| `./postgres-app/data/` | Application data | PostgreSQL binary files | postgres-app | postgres-app | ⭐⭐⭐ CRITICAL - Business data |
| `./pgadmin/` | pgAdmin settings | JSON config files | pgadmin | pgadmin | ⭐ Low - can recreate |
| `./airflow/dags/` | DAG definitions | `.py` files | All Airflow services | YOU | ⭐⭐⭐ CRITICAL - Your workflows |
| `./airflow/logs/` | All Airflow logs | `.log` files | YOU (debugging) | All Airflow services | ⭐⭐ Medium - debugging |
| `./airflow/plugins/` | Custom plugins | `.py` files | All Airflow services | YOU | ⭐⭐ Medium - custom code |
| `./airflow/scripts/` | Helper scripts | `.py`, `.sh` files | All Airflow services | YOU | ⭐⭐ Medium - utilities |
| `./spark_jobs/` | Spark applications | `.py` files | airflow-worker, pyspark-dev | YOU | ⭐⭐⭐ CRITICAL - Spark logic |
| `./pyspark_code/` | Jupyter notebooks | `.ipynb` files | pyspark-dev | YOU | ⭐⭐ Medium - development |
| `./checkpoint/` | Spark checkpoints | Binary checkpoint files | pyspark-dev | pyspark-dev | ⭐⭐ Medium - streaming state |
| `./data/` | Shared data files | `.csv`, `.parquet`, `.json` | pyspark-dev, python-dev | pyspark-dev, python-dev | ⭐⭐⭐ CRITICAL - processed data |
| `./python_code/` | Python apps/APIs | `.py` files | python-dev | YOU | ⭐⭐ Medium - application code |
| `./generators/` | Data generators | `.py` files | data-generator | YOU | ⭐ Low - test code |

---

## 📋 Complete Service Reference (Alternate View)

| Service | Code Location (Host) | Input Directory (Host → Container) | Output Directory (Container → Host) | Port (Host:Container) | Access URL | Purpose |
|---------|---------------------|------------------------------------|-------------------------------------|----------------------|------------|---------|
| **postgres-airflow** | N/A (database only) | `./postgres-airflow/data` → `/var/lib/postgresql/data` (DB files) | Same as input (DB files written here) | `5432:5432` | `localhost:5432` | Airflow metadata storage |
| **postgres-app** | N/A (database only) | `./postgres-app/data` → `/var/lib/postgresql/data` (DB files) | Same as input (DB files written here) | `5433:5432` | `localhost:5433` | Application data storage |
| **redis** | N/A (in-memory DB) | No persistent storage | No persistent storage | `6379:6379` | `localhost:6379` | Message broker for Celery |
| **pgadmin** | N/A (UI tool) | `./pgadmin` → `/var/lib/pgadmin` (settings) | Same as input (settings saved) | `5050:80` | `http://localhost:5050` | PostgreSQL management UI |
| **airflow-webserver** | `./airflow/dags/`<br>`./airflow/plugins/`<br>`./airflow/scripts/`<br>`./spark_jobs/` | `./airflow/dags` → `/opt/airflow/dags`<br>`./airflow/plugins` → `/opt/airflow/plugins`<br>`./airflow/scripts` → `/opt/airflow/scripts`<br>`./spark_jobs` → `/opt/airflow/spark_jobs` | `./airflow/logs` ← `/opt/airflow/logs`<br>`./spark_jobs` ← `/opt/airflow/spark_jobs` | `8080:8080` | `http://localhost:8080` | Airflow web UI |
| **airflow-scheduler** | Same as webserver | Same as webserver | Same as webserver | N/A | N/A | Schedules DAG runs |
| **airflow-worker** | Same as webserver | Same as webserver | Same as webserver | N/A | N/A | Executes task instances |
| **broker** | N/A (Kafka) | No persistent storage | No persistent storage | `9092:9092` | `localhost:9092` | Kafka message broker |
| **schema-registry** | N/A | No persistent storage | No persistent storage | `8081:8081` | `http://localhost:8081` | Kafka schema management |
| **kafka-ui** | N/A (UI tool) | No persistent storage | No persistent storage | `8090:8080` | `http://localhost:8090` | Kafka management UI |
| **pyspark-dev** | `./pyspark_code/`<br>`./spark_jobs/`<br>`./data/`<br>`./checkpoint/` | `./pyspark_code` → `/home/jovyan/work`<br>`./spark_jobs` → `/home/jovyan/spark_jobs`<br>`./data` → `/home/jovyan/data`<br>`./checkpoint` → `/home/jovyan/checkpoint` | `./pyspark_code` ← `/home/jovyan/work`<br>`./spark_jobs` ← `/home/jovyan/spark_jobs`<br>`./data` ← `/home/jovyan/data`<br>`./checkpoint` ← `/home/jovyan/checkpoint` | `8888:8888`<br>`4040:4040` | `http://localhost:8888?token=devtoken`<br>`http://localhost:4040` | Jupyter Lab with PySpark |
| **python-dev** | `./python_code/`<br>`./data/` | `./python_code` → `/app`<br>`./data` → `/data` | `./python_code` ← `/app`<br>`./data` ← `/data` | N/A | N/A | General Python development |
| **data-generator** | `./generators/` | `./generators` → `/app` | N/A (writes to Kafka) | N/A | N/A | Generate test data to Kafka |

---

## 📁 Summary of Key Folders

| Host Folder | Purpose | Used By |
|-------------|---------|---------|
| `./postgres-airflow/data/` | Airflow metadata database files | postgres-airflow |
| `./postgres-app/data/` | Application database files | postgres-app |
| `./pgadmin/` | pgAdmin settings | pgadmin |
| `./airflow/dags/` | Airflow DAG definitions | All Airflow services |
| `./airflow/logs/` | Airflow logs (scheduler, tasks, webserver) | All Airflow services |
| `./airflow/plugins/` | Custom Airflow plugins | All Airflow services |
| `./airflow/scripts/` | Helper scripts for DAGs | All Airflow services |
| `./spark_jobs/` | Spark application code | Airflow services, pyspark-dev |
| `./pyspark_code/` | Jupyter notebooks | pyspark-dev |
| `./checkpoint/` | Spark streaming checkpoints | pyspark-dev |
| `./data/` | General data files (CSV, Parquet, etc.) | pyspark-dev, python-dev |
| `./python_code/` | Python scripts/applications | python-dev |
| `./generators/` | Data generator scripts | data-generator |
