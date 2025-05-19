
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Python](https://img.shields.io/badge/python-3.9+-green)
![License](https://img.shields.io/badge/license-MIT-orange)

**A comprehensive system for extracting and managing data from scientific publications, focusing on figure captions and related entities.**



## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Expected Output](#-expected-output)
- [Docker Configuration](#-docker-configuration)
- [Configuration Options](#-configuration-options)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

## 🔍 Overview

The Scientific Publication Data Extraction System is designed to extract, process, and provide access to valuable metadata from scientific publications. The system focuses on extracting:

- 📑 Titles and abstracts
- 🖼️ Figure captions and URLs
- 🧬 Key entities (genes, diseases, etc.) mentioned in figure captions

Initially targeting PubMed Central (PMC) publications, the system architecture is designed to be extensible to additional data sources in the future.

## ✨ Features

<table>
<tr>
<td width="50%">

### Data Extraction
- ✅ Extract title and abstract
- ✅ Extract figure captions and URLs
- ✅ Identify key entities in figure captions
- ✅ Support for multiple entity types (genes, diseases, chemicals, etc.)

### Storage
- ✅ Store extracted data in DuckDB
- ✅ Efficient querying capabilities
- ✅ Data export in JSON and CSV formats
- ✅ Automatic database backups

</td>
<td width="50%">

### Access Methods
- ✅ RESTful API for programmatic access
- ✅ Command-line interface for management
- ✅ Watched folder for automatic processing

### Deployment
- ✅ Local installation
- ✅ Docker containerization
- ✅ Scalable architecture
- ✅ Comprehensive logging and monitoring

</td>
</tr>
</table>

## 🛠️ Requirements

- **Python 3.9+**
- **Windows 11** (or other OS with appropriate adjustments)
- **Docker** (optional, for containerized deployment)
- **Internet connection** (for accessing external APIs)

## 📥 Installation

### Option 1: Local Setup 💻

<div style="background-color: #f8f9fa; padding: 15px; border-radius: 5px; margin-bottom: 20px;">

1️⃣ **Clone the repository**
```bash
git clone https://github.com/your-org/scientific-publication-extraction.git
cd scientific-publication-extraction
```

2️⃣ **Set up a virtual environment**

```shellscript
python -m venv venv
venv\Scripts\activate  # On Windows
# source venv/bin/activate  # On Unix/Linux
```

3️⃣ **Install dependencies**

```shellscript
pip install -r requirements.txt
```

4️⃣ **Run the setup script**

```shellscript
python setup_windows.py
```

5️⃣ **Configure the application**

- Create a `.env` file based on `.env.example`
- Adjust settings as needed


`</div>`### Option 2: Docker Setup 🐳

`<div style="background-color: #f8f9fa; padding: 15px; border-radius: 5px; margin-bottom: 20px;">`1️⃣ **Clone the repository**

```shellscript
git clone https://github.com/your-org/scientific-publication-extraction.git
cd scientific-publication-extraction
```

2️⃣ **Build and start the Docker containers**

```shellscript
docker-compose build
docker-compose up -d
```

3️⃣ **Verify the installation**

```shellscript
docker-compose ps
```

`</div>`## 🚀 Usage

### Command Line Interface 💻

The system provides a comprehensive command-line interface for data processing and management.

`<details>
<summary><b>Process Papers</b></summary>`Process papers to extract figure captions and entities:

```shellscript
# Process a single paper
python -m src.cli.main process PMC6267067

# Process multiple papers
python -m src.cli.main process PMC6267067 PMC6267068

# Process papers from a file
python -m src.cli.main process --file papers.txt

# Process papers and wait for completion
python -m src.cli.main process PMC6267067 --wait
```

`</details>``<details>
<summary><b>Export Data</b></summary>`Export extracted data in JSON or CSV format:

```shellscript
# Export papers to JSON
python -m src.cli.main export papers --format json --output papers.json

# Export figures to CSV
python -m src.cli.main export figures --format csv --output figures.csv

# Export entities to JSON
python -m src.cli.main export entities --format json --output entities.json
```

`</details>``<details>
<summary><b>Configure System</b></summary>`Configure system settings:

```shellscript
# Show current configuration
python -m src.cli.main config show

# Update configuration
python -m src.cli.main config set api_port 8080

# Reset configuration to defaults
python -m src.cli.main config reset
```

`</details>``<details>
<summary><b>Watch Folders</b></summary>`Watch folders for files containing paper IDs:

```shellscript
# Start watching folders
python -m src.cli.main watch

# Start watching specific folders
python -m src.cli.main watch --folders data/input1 data/input2
```

`</details>`### REST API 🌐

The system provides a RESTful API for programmatic access to the extracted data.

`<details>
<summary><b>Start the API Server</b></summary>````shellscript
python -m src.cli.main serve
```

The API server will start on [http://localhost:8000](http://localhost:8000) by default.

`</details>``<details>
<summary><b>API Endpoints</b></summary>`#### Authentication

- `POST /api/v1/auth/token` - Get API token with credentials


#### Papers

- `POST /api/v1/papers` - Submit paper IDs for processing
- `GET /api/v1/papers` - List all processed papers
- `GET /api/v1/papers/{paper_id}` - Get details for specific paper
- `GET /api/v1/papers/{paper_id}/figures` - Get figures for specific paper


#### Figures

- `GET /api/v1/figures` - List all figures
- `GET /api/v1/figures/{figure_id}` - Get specific figure details
- `GET /api/v1/figures/{figure_id}/entities` - Get entities for specific figure


#### Entities

- `GET /api/v1/entities` - List all entities
- `GET /api/v1/entities/{type}` - List entities of specific type


#### Jobs

- `GET /api/v1/jobs` - List all jobs
- `GET /api/v1/jobs/{job_id}` - Get specific job status
- `POST /api/v1/jobs/{job_id}/cancel` - Cancel specific job


#### Export

- `GET /api/v1/export/papers` - Export papers data (CSV/JSON)
- `GET /api/v1/export/figures` - Export figures data (CSV/JSON)
- `GET /api/v1/export/entities` - Export entities data (CSV/JSON)


#### Admin

- `GET /api/v1/admin/config` - Get current configuration
- `PUT /api/v1/admin/config` - Update configuration
- `GET /api/v1/admin/stats` - Get system statistics


`</details>``<details>
<summary><b>API Authentication</b></summary>`The API uses API key authentication. Include the API key in the `X-API-Key` header:

```shellscript
curl -H "X-API-Key: your-api-key" http://localhost:8000/api/v1/papers
```

`</details>``<details>
<summary><b>API Examples</b></summary>`#### Submit papers for processing

```shellscript
curl -X POST -H "X-API-Key: your-api-key" -H "Content-Type: application/json" \
  -d '{"paper_ids": ["PMC6267067", "PMC6267068"]}' \
  http://localhost:8000/api/v1/papers
```

#### Get processed papers

```shellscript
curl -H "X-API-Key: your-api-key" http://localhost:8000/api/v1/papers
```

#### Get figures for a paper

```shellscript
curl -H "X-API-Key: your-api-key" \
  http://localhost:8000/api/v1/papers/PMC6267067/figures
```

#### Export entities as CSV

```shellscript
curl -H "X-API-Key: your-api-key" \
  http://localhost:8000/api/v1/export/entities?format=csv > entities.csv
```

`</details>`### Watched Folder 📁

The system can automatically process files placed in specified directories:

1. Place a text file containing paper IDs (one per line) in a watched folder (default: `data/input`).
2. The system will automatically detect the file and process the paper IDs.
3. Processing results will be available via the CLI or API.


## 📊 Expected Output

### Paper Data

```json
{
  "id": "PMC6267067",
  "title": "Example Paper Title",
  "abstract": "This is an example abstract...",
  "processed_date": "2023-05-19T14:30:45",
  "source": "PMC",
  "status": "completed",
  "error_message": null
}
```

### Figure Data

```json
{
  "id": "f1",
  "paper_id": "PMC6267067",
  "figure_number": 1,
  "caption": "Figure 1: Example figure caption describing the experiment...",
  "url": "https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6267067/figure/fig1/"
}
```

### Entity Data

```json
{
  "id": "e1",
  "figure_id": "f1",
  "entity_text": "TP53",
  "entity_type": "gene",
  "start_position": 42,
  "end_position": 46,
  "external_id": "7157"
}
```

## 🐳 Docker Configuration

The system includes Docker and Docker Compose configurations for easy deployment.

### Docker Compose Services

`<table>
<tr>
<th>Service</th>
<th>Purpose</th>
<th>Ports</th>
</tr>
<tr>
<td><b>app</b></td>
<td>Main application container for CLI commands</td>
<td>N/A</td>
</tr>
<tr>
<td><b>api</b></td>
<td>API server container</td>
<td>8000:8000</td>
</tr>
<tr>
<td><b>worker</b></td>
<td>Worker container for background processing</td>
<td>N/A</td>
</tr>
<tr>
<td><b>redis</b></td>
<td>Redis container for task queue</td>
<td>6379:6379</td>
</tr>
</table>`### Docker Compose Commands

```shellscript
# Build containers
docker-compose build

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Run CLI commands
docker-compose run app process PMC6267067

# Stop all services
docker-compose down
```

### Docker Environment Variables

Docker environment variables can be set in the `docker-compose.yml` file:

```yaml
environment:
  - LOG_LEVEL=INFO
  - API_HOST=0.0.0.0
  - API_PORT=8000
  - DUCKDB_PATH=/app/data/db/publications.duckdb
```

## ⚙️ Configuration Options

The system can be configured using environment variables or a `.env` file.

`<details>
<summary><b>General Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `APP_NAME` | Application name | Scientific Publication Data Extraction
| `ENVIRONMENT` | Development, testing, or production | DEVELOPMENT
| `LOG_LEVEL` | DEBUG, INFO, WARNING, ERROR, CRITICAL | INFO
| `TEMP_DIR` | Directory for temporary files | data/temp


`</details>``<details>
<summary><b>API Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `API_HOST` | Host to bind API server | 0.0.0.0
| `API_PORT` | Port to bind API server | 8000
| `API_WORKERS` | Number of worker processes | 4
| `ENABLE_DOCS` | Enable/disable API documentation | True


`</details>``<details>
<summary><b>Security Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `AUTH_ENABLED` | Enable/disable authentication | True
| `AUTH_METHOD` | "api_key" or "password" | api_key
| `API_KEYS` | List of valid API keys | []
| `TOKEN_EXPIRATION` | Token expiration time in seconds | 3600


`</details>``<details>
<summary><b>Storage Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `STORAGE_TYPE` | "duckdb" (default, expandable) | duckdb
| `DUCKDB_PATH` | Path to DuckDB file | data/db/publications.duckdb
| `BACKUP_ENABLED` | Enable/disable backups | True
| `BACKUP_INTERVAL` | Backup interval in hours | 24


`</details>``<details>
<summary><b>Processing Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `EXTRACTION_WORKERS` | Number of extraction workers | 4
| `ENTITY_DETECTION_WORKERS` | Number of entity detection workers | 4
| `BATCH_SIZE` | Number of papers to process in a batch | 10
| `RETRY_LIMIT` | Number of retries for failed API calls | 3
| `RETRY_DELAY` | Delay between retries in seconds | 5


`</details>``<details>
<summary><b>Watched Folder Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `WATCHED_FOLDERS` | List of folders to watch | ["data/input"]
| `WATCH_INTERVAL` | Interval to check folders in seconds | 10
| `FILE_PATTERNS` | List of file patterns to process | ["*.txt"]


`</details>``<details>
<summary><b>External API Settings</b></summary>`| Setting | Description | Default
|-----|-----|-----
| `BIOC_PMC_URL` | URL for BioC-PMC API | [https://www.ncbi.nlm.nih.gov/research/bionlp/RESTful/pmcoa.cgi](https://www.ncbi.nlm.nih.gov/research/bionlp/RESTful/pmcoa.cgi)
| `BIOC_PMC_RATE_LIMIT` | Rate limit for BioC-PMC API | 10
| `PUBTATOR3_URL` | URL for PubTator3 API | [https://www.ncbi.nlm.nih.gov/research/pubtator3/api](https://www.ncbi.nlm.nih.gov/research/pubtator3/api)
| `PUBTATOR3_RATE_LIMIT` | Rate limit for PubTator3 API | 10


`</details>`## 🔧 Troubleshooting

### Common Issues and Solutions

`<table>
<tr>
<th>Issue</th>
<th>Possible Causes</th>
<th>Solutions</th>
</tr>
<tr>
<td>API Server Not Starting</td>
<td>
• Port already in use<br>
• Missing dependencies<br>
• Configuration errors
</td>
<td>
• Check if another process is using the port<br>
• Verify all dependencies are installed<br>
• Check configuration file for errors
</td>
</tr>
<tr>
<td>Database Connection Error</td>
<td>
• Database file not found<br>
• Permission issues<br>
• Corrupted database file
</td>
<td>
• Verify database file path<br>
• Check file permissions<br>
• Restore from backup if corrupted
</td>
</tr>
<tr>
<td>Paper Processing Failures</td>
<td>
• External API unavailable<br>
• Rate limiting<br>
• Invalid paper ID<br>
• Timeout
</td>
<td>
• Check external API status<br>
• Adjust rate limiting settings<br>
• Verify paper ID format<br>
• Increase timeout settings
</td>
</tr>
<tr>
<td>High Memory Usage</td>
<td>
• Processing too many papers simultaneously<br>
• Memory leaks<br>
• Large response data
</td>
<td>
• Adjust batch size<br>
• Check for memory leaks<br>
• Implement pagination for large responses
</td>
</tr>
<tr>
<td>Slow API Response</td>
<td>
• Database queries not optimized<br>
• High system load<br>
• Network latency
</td>
<td>
• Optimize database queries<br>
• Scale resources<br>
• Implement caching
</td>
</tr>
</table>`### Logs

Check the logs for detailed error information:

```shellscript
# View application logs
cat logs/app.log

# View API server logs
cat logs/api.log

# View worker logs
cat logs/worker.log
```

### Health Check

The system provides a health check endpoint to verify the status of all components:

```shellscript
curl http://localhost:8000/api/v1/health
```

## 👥 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request


## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

`<div align="center">
<p>Made with ❤️ for scientific research</p>
</div>
````I've provided two versions of the README.md file for your Scientific Publication Data Extraction System:

1. **Standard Version**: A comprehensive, well-structured README with all the necessary information about installation, usage, configuration, and troubleshooting.
2. **Enhanced Version**: A more visually appealing README with emojis, badges, collapsible sections, tables, and better formatting while maintaining the same core information.


Both versions include:

- Project overview and purpose
- Features and capabilities
- Installation instructions (local and Docker)
- Usage examp- les for CLI, API, and watched folder
- Expected output formats
- Docker configuration and commands
- Configuration options
- Troubleshooting tips

You can choose the version that best fits your project's style and audience.

`<Actions>
  <Action name="Create a quick start guide" description="Create a condensed quick start guide for new users" />
  <Action name="Add API examples with curl" description="Add more API usage examples with curl commands" />
  <Action name="Create a deployment guide" description="Create a detailed deployment guide for production environments" />
  <Action name="Add architecture diagram" description="Add a system architecture diagram to the README" />
  <Action name="Create a user manual" description="Create a comprehensive user manual with all features explained" />
</Actions>`
