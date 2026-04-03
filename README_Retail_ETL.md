# 🔄 Retail ETL Pipeline

A comprehensive **Extract, Transform, Load (ETL)** pipeline for processing and analyzing retail transaction data. This project demonstrates data engineering best practices using Python, SQL, and data warehousing techniques to transform raw e-commerce data into actionable insights.

## 📋 Table of Contents

- [Overview](#overview)
- [Project Architecture](#project-architecture)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Dataset](#dataset)
- [ETL Process](#etl-process)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [SQL Queries](#sql-queries)
- [Results & Insights](#results--insights)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## 🎯 Overview

This project implements a complete ETL pipeline for retail/e-commerce data, transforming raw transaction records into a structured data warehouse. The pipeline handles data extraction from Excel files, performs comprehensive data cleaning and transformation using Python (pandas), and loads the processed data into a SQL database for analysis and reporting.

### Business Problem

Retail businesses generate massive amounts of transactional data daily. This project addresses:
- **Data Quality Issues**: Missing values, duplicates, and inconsistencies
- **Data Integration**: Consolidating data from multiple sources
- **Analytics Readiness**: Preparing data for business intelligence and reporting
- **Scalability**: Building a reusable pipeline for regular data processing

### Objectives

- ✅ Extract data from multiple sources (Excel, CSV, databases)
- ✅ Clean and validate data to ensure quality
- ✅ Transform data into a structured warehouse schema
- ✅ Load data efficiently into a relational database
- ✅ Enable advanced analytics and reporting

## 🏗️ Project Architecture

```
┌─────────────────┐
│  Data Sources   │
│  (Excel/CSV)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   EXTRACTION    │
│  - Read files   │
│  - Validate     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ TRANSFORMATION  │
│  - Clean data   │
│  - Handle nulls │
│  - Enrich data  │
│  - Validate     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│     LOADING     │
│  - SQL Database │
│  - Data Mart    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   ANALYTICS &   │
│   REPORTING     │
└─────────────────┘
```

## ✨ Features

### Data Extraction
- 📥 Read data from Excel and CSV files
- 🔍 Automatic schema detection
- 📊 Support for multiple data formats

### Data Transformation
- 🧹 **Data Cleaning**: 
  - Remove duplicates
  - Handle missing values
  - Standardize formats
- 🔄 **Data Enrichment**:
  - Calculate derived fields (Total Amount, Customer Lifetime Value)
  - Create date dimensions (Year, Month, Quarter)
  - Product categorization
- ✅ **Data Validation**:
  - Type checking
  - Range validation
  - Business rule enforcement

### Data Loading
- 💾 Load to SQL database (MySQL/PostgreSQL)
- 🏢 Star schema implementation
- 📈 Optimized for analytical queries
- 🔐 Transaction management and error handling

### Analytics
- 📊 Pre-built SQL queries for business insights
- 💡 Customer segmentation analysis
- 📈 Sales trend analysis
- 🛍️ Product performance metrics

## 🛠️ Technologies Used

| Category | Technology |
|----------|-----------|
| **Programming Language** | Python 3.8+ |
| **Data Processing** | pandas, NumPy |
| **Database** | MySQL / PostgreSQL / SQLite |
| **Data Analysis** | Jupyter Notebook |
| **SQL** | SQL DDL & DML |
| **Data Source** | Excel (xlsx), CSV |
| **Development** | Git, GitHub |

### Python Libraries
```python
pandas          # Data manipulation
numpy           # Numerical operations
sqlalchemy      # Database connectivity
pymysql         # MySQL connector
openpyxl        # Excel file handling
datetime        # Date operations
```

## 📊 Dataset

### Source
**Online Retail Dataset** (`Online Retail.xlsx`)

### Schema
The dataset contains the following fields:

| Column | Description | Data Type |
|--------|-------------|-----------|
| InvoiceNo | Unique transaction identifier | String |
| StockCode | Product code | String |
| Description | Product description | String |
| Quantity | Number of items purchased | Integer |
| InvoiceDate | Transaction date and time | DateTime |
| UnitPrice | Price per unit | Float |
| CustomerID | Unique customer identifier | Integer |
| Country | Customer's country | String |

### Dataset Characteristics
- **Records**: 500,000+ transactions
- **Time Period**: Multiple years of retail data
- **Geography**: International transactions
- **Categories**: Various product categories

## 🔄 ETL Process

### 1. Extract Phase

```python
# Load data from Excel
df = pd.read_excel('Online Retail.xlsx')

# Initial data profiling
print(df.info())
print(df.describe())
```

**Tasks:**
- Read source files
- Initial data profiling
- Identify data quality issues

### 2. Transform Phase

```python
# Data Cleaning
# Remove duplicates
df = df.drop_duplicates()

# Handle missing values
df = df.dropna(subset=['CustomerID'])
df['Description'].fillna('Unknown', inplace=True)

# Data Enrichment
# Calculate total amount
df['TotalAmount'] = df['Quantity'] * df['UnitPrice']

# Extract date components
df['Year'] = df['InvoiceDate'].dt.year
df['Month'] = df['InvoiceDate'].dt.month
df['Quarter'] = df['InvoiceDate'].dt.quarter

# Data Validation
# Remove negative quantities and prices
df = df[(df['Quantity'] > 0) & (df['UnitPrice'] > 0)]
```

**Transformations Applied:**
- ✅ Duplicate removal
- ✅ Missing value imputation
- ✅ Data type conversion
- ✅ Feature engineering
- ✅ Outlier detection and handling
- ✅ Data standardization

### 3. Load Phase

```sql
-- Create dimension and fact tables
-- Load transformed data into SQL database
-- Create indexes for optimization
```

**Loading Strategy:**
- Dimensional modeling (Star Schema)
- Fact and dimension tables
- Incremental loading support
- Error logging and recovery

## 🚀 Installation & Setup

### Prerequisites

- Python 3.8 or higher
- MySQL/PostgreSQL/SQLite (choose one)
- Jupyter Notebook
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/vinaysai7/Retail_ETL_Pipeline.git
cd Retail_ETL_Pipeline
```

### Step 2: Create Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows
venv\Scripts\activate
# On macOS/Linux
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install pandas numpy sqlalchemy pymysql openpyxl jupyter
```

### Step 4: Database Setup

```bash
# For MySQL
mysql -u root -p < retail_etl.sql

# Or configure your database connection in the notebook
```

### Step 5: Configure Database Connection

Update the database connection settings in the Jupyter notebook:

```python
from sqlalchemy import create_engine

# MySQL
engine = create_engine('mysql+pymysql://username:password@localhost/retail_db')

# PostgreSQL
engine = create_engine('postgresql://username:password@localhost/retail_db')

# SQLite (for testing)
engine = create_engine('sqlite:///retail.db')
```

## 💻 Usage

### Run the ETL Pipeline

1. **Start Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

2. **Open the Pipeline Notebook**
   - Navigate to `Untitled.ipynb`
   - Run cells sequentially

3. **Execute ETL Steps**
   - Extract: Load source data
   - Transform: Apply transformations
   - Load: Insert into database

4. **Run Analytics Queries**
   - Execute SQL scripts from `retail_etl.sql`
   - Generate reports and insights

### Example Workflow

```python
# 1. Extract
df = pd.read_excel('Online Retail.xlsx')

# 2. Transform
df_clean = clean_data(df)
df_enriched = enrich_data(df_clean)

# 3. Load
df_enriched.to_sql('fact_sales', engine, if_exists='append', index=False)

# 4. Analyze
results = pd.read_sql("SELECT * FROM sales_summary", engine)
```

## 📁 Project Structure

```
Retail_ETL_Pipeline/
│
├── Online Retail.xlsx              # Source data file
├── Untitled.ipynb                  # Main ETL pipeline notebook
├── retail_etl.sql                  # SQL scripts for database setup & queries
├── Retail_ETL_Pipeline_Project.pdf # Project documentation
├── README.md                       # Project documentation (this file)
├── requirements.txt                # Python dependencies (to be created)
│
├── data/                           # Data directory (optional)
│   ├── raw/                       # Raw data files
│   ├── processed/                 # Cleaned data
│   └── output/                    # Final outputs
│
├── scripts/                        # Additional scripts (optional)
│   ├── extract.py                 # Extraction module
│   ├── transform.py               # Transformation module
│   └── load.py                    # Loading module
│
└── logs/                          # Logs directory (optional)
    └── etl_pipeline.log           # Pipeline execution logs
```

## 🗄️ SQL Queries

The `retail_etl.sql` file contains analytical queries for:

### Sales Analysis
```sql
-- Top selling products
SELECT StockCode, Description, SUM(Quantity) as TotalSold
FROM fact_sales
GROUP BY StockCode, Description
ORDER BY TotalSold DESC
LIMIT 10;
```

### Customer Analysis
```sql
-- Customer lifetime value
SELECT CustomerID, 
       COUNT(DISTINCT InvoiceNo) as TotalOrders,
       SUM(TotalAmount) as LifetimeValue
FROM fact_sales
GROUP BY CustomerID
ORDER BY LifetimeValue DESC;
```

### Time-Series Analysis
```sql
-- Monthly revenue trend
SELECT Year, Month, SUM(TotalAmount) as MonthlyRevenue
FROM fact_sales
GROUP BY Year, Month
ORDER BY Year, Month;
```

### Geographic Analysis
```sql
-- Sales by country
SELECT Country, 
       COUNT(*) as Orders,
       SUM(TotalAmount) as Revenue
FROM fact_sales
GROUP BY Country
ORDER BY Revenue DESC;
```

## 📈 Results & Insights

The ETL pipeline enables the following business insights:

### Key Metrics
- 📊 **Total Revenue**: $X million
- 🛒 **Total Orders**: XXX,XXX transactions
- 👥 **Unique Customers**: XX,XXX customers
- 🌍 **Countries Served**: XX countries

### Business Insights
1. **Top Products**: Identification of best-selling items
2. **Customer Segmentation**: High-value vs. regular customers
3. **Seasonal Trends**: Peak sales periods identified
4. **Geographic Patterns**: High-performing regions
5. **Revenue Trends**: Growth patterns over time

*Detailed analysis available in `Retail_ETL_Pipeline_Project.pdf`*

## 🔮 Future Enhancements

### Planned Features
- [ ] **Automation**: Schedule pipeline with Apache Airflow
- [ ] **Cloud Migration**: Move to AWS/Azure cloud platforms
- [ ] **Real-time Processing**: Implement streaming ETL with Kafka
- [ ] **Data Quality Monitoring**: Add data validation framework
- [ ] **Dashboard**: Create interactive Power BI/Tableau dashboards
- [ ] **Machine Learning**: Add predictive analytics models
- [ ] **API Integration**: REST API for data access
- [ ] **Docker**: Containerize the application
- [ ] **CI/CD**: Implement automated testing and deployment

### Scalability Improvements
- Parallel processing with Dask or PySpark
- Incremental loading strategies
- Partitioning for large datasets
- Data lake integration

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### Contribution Ideas
- Add more data sources (APIs, databases)
- Implement additional transformations
- Create visualization dashboards
- Add unit tests
- Improve documentation
- Optimize performance

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- UCI Machine Learning Repository for the Online Retail dataset
- The data engineering community for best practices
- Open source contributors of pandas, SQLAlchemy, and other libraries

## 📚 Resources

- [ETL Best Practices](https://www.kimballgroup.com/)
- [Pandas Documentation](https://pandas.pydata.org/)
- [SQLAlchemy Documentation](https://www.sqlalchemy.org/)
- [Star Schema Design](https://en.wikipedia.org/wiki/Star_schema)

## 👤 Contact

**Vinay Sai**

- GitHub: [@vinaysai7](https://github.com/vinaysai7)
- Project Link: [https://github.com/vinaysai7/Retail_ETL_Pipeline](https://github.com/vinaysai7/Retail_ETL_Pipeline)

---

### ⭐ If you found this project helpful, please consider giving it a star!

---

**Tags**: `ETL` `Data Engineering` `Python` `SQL` `Data Warehouse` `pandas` `MySQL` `Data Pipeline` `Retail Analytics` `Business Intelligence`
