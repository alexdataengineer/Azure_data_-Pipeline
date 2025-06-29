# World Bank Population Data Pipeline

**Developed by: Alexsander Silveira**

## Project Overview

I developed this Azure Data Pipeline to extract, transform, and load population data from the World Bank API into a structured data lake architecture. This project demonstrates a complete end-to-end data engineering solution using Azure Synapse Analytics, implementing the modern data architecture pattern with Bronze, Silver, and Gold layers.

The pipeline specifically focuses on Brazil's population data (indicator SP.POP.TOTL) and transforms it into decade-based aggregations that provide valuable insights for demographic analysis and business intelligence.

## What This Pipeline Does

This data pipeline extracts population statistics from the World Bank API and processes them through a three-layer data architecture:

1. **Bronze Layer**: Raw data ingestion from the World Bank API
2. **Silver Layer**: Data cleaning and standardization
3. **Gold Layer**: Business-ready aggregations and analytics

The final output is an external table in Azure Synapse SQL Serverless that's ready for Power BI consumption, enabling real-time analytics and reporting.

## Data Flow Architecture

### Step 1: Bronze Layer - Data Ingestion
I structured the project to begin with the `bronze_data` pipeline, which:
- Connects to the World Bank API using a REST linked service
- Extracts population data for Brazil using the endpoint: `country/bra/indicator/SP.POP.TOTL`
- Stores raw JSON data in Azure Data Lake Storage with timestamped filenames
- Implements pagination support for handling large datasets

### Step 2: Silver Layer - Data Transformation
The `data_transformation_silver` pipeline processes the raw data through:
- A PySpark notebook that reads JSON files from the bronze layer
- Data filtering to remove null values and ensure data quality
- Schema transformation to create a clean, structured dataset with columns:
  - `pais` (country)
  - `iso3` (ISO3 country code)
  - `ano` (year)
  - `populacao_total` (total population)
- Output to Parquet format in the silver layer for optimal performance

### Step 3: Gold Layer - Business Intelligence
The `gold_layer` pipeline creates business-ready analytics:
- Calculates year-over-year population growth metrics
- Aggregates data by decades for trend analysis
- Generates key metrics including:
  - Average population per decade
  - Maximum and minimum population values per decade
  - Number of available years per decade
- Creates the final dataset ready for external table creation

### Step 4: External Table Creation
The pipeline concludes with SQL script execution that:
- Creates an external data source pointing to the gold layer
- Defines a Parquet file format for efficient data access
- Establishes an external table `worldbank_populacao_por_decada` in Synapse SQL Serverless
- Makes the data immediately available for Power BI and other analytics tools

## Data Source: World Bank API

The pipeline extracts data from the World Bank's comprehensive database, specifically focusing on:
- **Country**: Brazil (BRA)
- **Indicator**: Total Population (SP.POP.TOTL)
- **Format**: JSON with historical time series data
- **Frequency**: Annual population statistics

This data source provides reliable, internationally recognized demographic information that serves as a foundation for various analytical use cases.

## Business Benefits and ROI

This type of population data pipeline generates significant business value through:

### Market Sizing and Opportunity Assessment
- **Demographic Analysis**: Understanding population trends helps identify market opportunities and customer segments
- **Geographic Expansion**: Population data supports decisions about market entry and resource allocation
- **Product Development**: Demographic insights inform product design and marketing strategies

### Strategic Decision Making
- **Investment Planning**: Population trends guide infrastructure and capacity planning
- **Risk Assessment**: Demographic changes help identify potential market risks and opportunities
- **Competitive Intelligence**: Understanding population dynamics provides competitive advantages

### Operational Efficiency
- **Resource Allocation**: Population data optimizes distribution networks and service delivery
- **Capacity Planning**: Demographic trends inform staffing and facility requirements
- **Performance Measurement**: Population metrics serve as denominators for per-capita performance indicators

### Financial Impact
- **Revenue Optimization**: Better demographic targeting improves marketing ROI
- **Cost Reduction**: Data-driven decisions reduce operational inefficiencies
- **Risk Mitigation**: Population insights help avoid investments in declining markets

## Technical Implementation

I designed this pipeline using Azure Synapse Analytics with the following components:

### Pipeline Orchestration
- **Main Pipeline**: `execultion_ordem` orchestrates the entire data flow
- **Dependencies**: Sequential execution ensures data quality and consistency
- **Error Handling**: Built-in retry mechanisms and timeout configurations

### Data Processing
- **Apache Spark**: PySpark notebooks for scalable data transformation
- **Data Lake Storage**: Hierarchical storage with bronze/silver/gold layers
- **Parquet Format**: Columnar storage for optimal query performance

### Data Consumption
- **External Tables**: SQL Serverless integration for seamless analytics
- **Power BI Ready**: Direct connectivity for real-time reporting
- **Scalable Architecture**: Supports growing data volumes and user demands

## Project Structure

```
Azure_data_-Pipeline/
├── pipeline/                    # Azure Data Factory pipelines
│   ├── bronze_data.json        # Raw data ingestion
│   ├── data_transformation_silver.json  # Data cleaning
│   ├── gold_layer.json         # Business aggregations
│   └── execultion_ordem.json   # Pipeline orchestration
├── notebook/                    # PySpark transformation notebooks
│   ├── data_transformation_silver.json
│   └── gold_layer.json
├── dataset/                     # Data source definitions
│   ├── RestResource1.json      # World Bank API connection
│   └── Json1.json              # JSON sink configuration
├── sqlscript/                   # External table definitions
│   └── external_table_report_worldbank_populacao_por_decada.json
├── linkedService/               # Connection configurations
├── credential/                  # Authentication settings
├── trigger/                     # Pipeline execution triggers
└── integrationRuntime/          # Data processing runtime
```

## Explore the Code

To better understand the implementation details and transformation logic, I encourage you to explore:

1. **The PySpark notebooks** in the `notebook/` directory to see the actual data transformation code
2. **Pipeline configurations** in the `pipeline/` directory to understand the orchestration flow
3. **SQL scripts** in the `sqlscript/` directory to see how the external table is created
4. **Dataset definitions** to understand the data source connections

Each component is designed to be modular and reusable, making it easy to extend this pipeline for additional countries, indicators, or analytical requirements.

## Getting Started

To deploy and run this pipeline:
1. Import the pipeline definitions into your Azure Synapse workspace
2. Configure the linked services with appropriate credentials
3. Set up the required Azure Data Lake Storage containers
4. Execute the main orchestration pipeline to begin data processing

The pipeline is designed to run on a schedule, continuously updating the population data and maintaining the external table for real-time analytics consumption.