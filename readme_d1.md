Scientific Publication Data Extraction System
A modular, extensible system for extracting and managing structured data from scientific publications—focused on figure captions and key entities like genes. Built with Python, FastAPI, Typer, DuckDB, and Celery, it offers both API and CLI interfaces.

🚀 1. System Overview
The system is designed to:

Extract:

Title

Abstract

Figure captions

Figure URLs (if available)

Entities (e.g., genes) from figure captions

Store metadata in DuckDB

Support CLI, API, and folder ingestion modes

Scale using Celery task queues

Authenticate access securely

Initially targets PubMed Central but can be extended to other sources.

🏗️ 2. Architecture
A layered architecture:

Client Layer: CLI, REST API, Watched Folder

Application Layer: Authentication, Core Logic, Configuration

Service Layer: Extraction, Entity Detection, Storage

Integration Layer: BioC-PMC, PubTator3, DuckDB

Mermaid Diagram:

mermaid
Copy
Edit
graph TD
    A["Client Layer"] --> B["Application Layer"]
    B --> C["Service Layer"]
    C --> D["Integration Layer"]

    subgraph "Client Layer"
    A1["CLI"] 
    A2["REST API"]
    A3["Watched Folder"]
    end

    subgraph "Application Layer"
    B1["Authentication"]
    B2["Core Logic"]
    B3["Config Management"]
    end

    subgraph "Service Layer"
    C1["Extraction Service"]
    C2["Entity Detection"]
    C3["Storage Service"]
    end

    subgraph "Integration Layer"
    D1["BioC-PMC Client"]
    D2["PubTator3 Client"]
    D3["DuckDB Client"]
    end

    A1 --> B2
    A2 --> B1
    A2 --> B2
    A3 --> B2
    B1 --> B2
    B2 --> C1
    B2 --> C2
    B2 --> C3
    B3 --> B2
    C1 --> D1
    C2 --> D2
    C3 --> D3
🧩 3. Data Models
📝 Paper

json
Copy
Edit
{
  "id": "string",
  "title": "string",
  "abstract": "string",
  "processed_date": "datetime",
  "source": "PMC",
  "status": "pending|processing|completed|failed",
  "error_message": "string|null"
}
🖼️ Figure

json
Copy
Edit
{
  "id": "string",
  "paper_id": "string",
  "figure_number": "int",
  "caption": "string",
  "url": "string|null"
}
🧬 Entity

json
Copy
Edit
{
  "id": "string",
  "figure_id": "string",
  "entity_text": "string",
  "entity_type": "gene|disease|...",
  "start_position": "int",
  "end_position": "int",
  "external_id": "string"
}
🛠️ Job

json
Copy
Edit
{
  "id": "string",
  "job_type": "string",
  "status": "queued|processing|completed|failed",
  "created_at": "datetime",
  "completed_at": "datetime",
  "paper_ids": ["PMC12345", "PMC67890"],
  "total_papers": "int",
  "processed_papers": "int",
  "failed_papers": "int"
}
🔄 4. Data Flow
📝 Ingestion Flow:

User submits paper(s) via CLI/API/watched folder

Job is created and queued

For each paper:

Extract metadata

Detect entities

Store in DuckDB

Status is updated

🔍 Query Flow:

Authenticated user queries via CLI/API

Data is fetched from DuckDB

Result is returned in JSON/CSV

⚙️ 5. Technical Stack
Component	Technology
Language	Python 3.9+
Web Framework	FastAPI
CLI Framework	Typer
DB Engine	DuckDB
ORM	SQLAlchemy
Task Queue	Celery + Redis
HTTP Client	httpx
Container	Docker
Tests	pytest
Docs	Sphinx (RTD)

📡 6. API Endpoints
Authentication

POST /api/v1/auth/token

Papers

POST /api/v1/papers

GET /api/v1/papers

GET /api/v1/papers/{paper_id}

GET /api/v1/papers/{paper_id}/figures

Figures

GET /api/v1/figures

GET /api/v1/figures/{figure_id}

GET /api/v1/figures/{figure_id}/entities

Entities

GET /api/v1/entities

GET /api/v1/entities/{type}

Jobs

GET /api/v1/jobs

GET /api/v1/jobs/{job_id}

POST /api/v1/jobs/{job_id}/cancel

Export

GET /api/v1/export/papers

GET /api/v1/export/figures

GET /api/v1/export/entities

Admin

GET /api/v1/admin/config

PUT /api/v1/admin/config

GET /api/v1/admin/stats

🧰 7. Configuration
Configurable via YAML or .env

General: app_name, environment, temp_dir, log_level

API: host, port, workers

Security: auth_enabled, method, api_keys

Storage: duckdb_path, backups

Processing: batch_size, retries, workers

Watched Folders: paths, file_patterns

APIs: rate_limits, base_urls

🔐 8. Security
API key + Token authentication

Hashed API key storage

No PII stored

Rate limiting

Input validation

Minimal permissions for containers

📈 9. Scalability
Stateless API = horizontal scaling

Celery task queues = distributed job handling

Batch processing + caching

Async I/O

Monitoring and retry logic

🧪 10. Testing
Use pytest for unit and integration tests

Mock external APIs for deterministic results

Test configuration and error paths

🧱 11. Extensibility
Plug-in system for data sources

Additional entity types via pluggable recognizers

Support for ML pipelines in entity detection

Full-text search and advanced filters

✅ 12. Conclusion
The Scientific Publication Data Extraction System is a scalable, modular platform for ingesting, processing, and querying publication metadata. Designed for reliability, security, and extensibility.
