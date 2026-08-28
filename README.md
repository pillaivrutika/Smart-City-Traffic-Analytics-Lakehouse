# TrafficVision

**Smart City Traffic Analytics Lakehouse on Azure**

TrafficVision is an end-to-end data engineering and analytics pipeline that ingests real-time and batch traffic sensor data, processes it through a medallion lakehouse architecture, and delivers interactive insights via Power BI. Built entirely on Azure, it demonstrates a production-style pattern for smart city IoT and traffic analytics.

## Architecture
Traffic Data (CSV / API Sensors)
        │
        ▼
Azure Data Factory (Ingestion & Orchestration)
        │
        ▼
ADLS Gen2 (Cloud Data Lake)
        │
        ▼
Azure Databricks (Apache Spark / PySpark)
        │
  Bronze → Silver → Gold  (Delta Lake)
        │
        ▼
Databricks SQL Warehouse (Analytics / SQL Serving)
        │
        ▼
Power BI (Interactive Dashboards)
```

| Layer | Service | Purpose |
|---|---|---|
| Ingestion | Azure Data Factory | Orchestrates and schedules ingestion pipelines from CSV files and sensor APIs |
| Storage | ADLS Gen2 | Durable, hierarchical cloud data lake for raw and processed data |
| Processing | Azure Databricks (Spark / PySpark) | Transforms data through the bronze, silver, and gold layers using Delta Lake |
| Serving | Databricks SQL Warehouse | Exposes curated gold-layer tables via SQL for analytics tools |
| Visualization | Power BI | Interactive dashboards for traffic volume, congestion, and trend analysis |

### Medallion layers

- **Bronze** — raw data landed as-is from source, in Delta format for reliability and schema evolution
- **Silver** — cleaned, deduplicated, and enriched data (e.g. joined with location and time dimensions)
- **Gold** — aggregated KPI marts optimized for BI consumption and fast querying

---

## Dashboards

The Power BI layer provides the following views:

- Traffic volume trends
- Congestion analysis
- Peak-hour trends
- Vehicle composition
- Direction analysis
- Location-based insights

---

## Tech stack

- **Orchestration:** Azure Data Factory
- **Storage:** Azure Data Lake Storage Gen2
- **Compute:** Azure Databricks (Apache Spark, PySpark, Delta Lake)
- **Serving:** Databricks SQL Warehouse
- **Visualization:** Power BI

---

## Getting started

### Prerequisites

- An Azure subscription with access to Data Factory, ADLS Gen2, and Databricks
- Power BI Desktop or Power BI Service access
- Traffic data source (CSV files or a live sensor API endpoint)

### Setup

1. **Provision storage** — create an ADLS Gen2 account with containers for `raw`, `bronze`, `silver`, and `gold`.
2. **Configure ingestion** — set up an Azure Data Factory pipeline to pull traffic data from CSV drops or the sensor API into the `raw` container on a schedule.
3. **Deploy Databricks jobs** — create notebooks/jobs for:
   - `bronze` — ingest raw files into Delta tables with minimal transformation
   - `silver` — clean, deduplicate, and enrich the bronze data
   - `gold` — aggregate into KPI marts (volume, congestion, peak hours, vehicle type, direction, location)
4. **Enable SQL serving** — expose the gold Delta tables through a Databricks SQL Warehouse.
5. **Connect Power BI** — use the native Databricks connector (or ODBC/JDBC) to connect Power BI to the SQL Warehouse and build/import the dashboards.

---

## Project structure

```
trafficvision/
├── adf/                # Azure Data Factory pipeline definitions (ARM/JSON)
├── notebooks/
│   ├── bronze/          # Raw ingestion notebooks
│   ├── silver/          # Cleaning & enrichment notebooks
│   └── gold/            # KPI aggregation notebooks
├── sql/                 # SQL Warehouse views and queries
├── powerbi/              # .pbix dashboard files
└── README.md
```

*(Adjust to match your actual repository layout.)*

---

## Roadmap

- [ ] Real-time streaming ingestion via Event Hubs / IoT Hub
- [ ] Automated data quality checks between layers
- [ ] CI/CD for Databricks jobs and ADF pipelines
- [ ] Alerting on congestion anomalies

---

## License

Specify your project's license here (e.g. MIT).

## Contributing

Contributions are welcome — please open an issue or submit a pull request.
