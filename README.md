# BAN6800 Data Analytics Capstone
# Milestone 1: Business Analytics Project-Ready Dataset
# LEANER ID: 162894
# Submitted to Dr. Raphael Wanjiku

🪙 1. Bronze Layer — Raw Ingestion
📌 Purpose

The Bronze layer stores raw, untransformed data ingested from SAP S/4HANA extract files through Databricks Auto Loader.

📥 Data Sources

Purchase Requisition (PR) raw extract

Purchase Order (PO) raw extract

Vendor & material attributes (optional)

⚙️ Processes

Auto Loader ingestion from ADLS

Schema application

Raw CSV → Delta Lake conversion

No transformations

No filtering

No business logic

📄 File Structure
/bronze/
   └── procurement/
       ├── raw_pr_po/
       ├── checkpoint/
       └── abc_dw_br_procurment_raw_data.delta

🧪 Quality Notes

Data is uncleaned and may contain:

duplicates

empty strings

incorrect SAP date formats (YYYYMMDD)

deletion indicators

inconsistent schema evolution

🥈 2. Silver Layer — Clean & Standardized Data
📌 Purpose

The Silver layer converts raw data into cleaned, lightly transformed, analytics-ready tables, still preserving row-level detail.

⚙️ Transformations Applied

Trimmed all string fields

Converted SAP date fields to Databricks DateType()

Removed deleted/cancelled documents

Normalized field naming conventions

Removed duplicates

Applied type casting

Split data into two normalized tables:

✔ PO Silver Table: abc_dw_sl_pur_ord
✔ PR Silver Table: abc_dw_sl_pr_req

📄 File Structure
/silver/
   └── procurement/
       ├── abc_dw_sl_pur_ord.delta
       ├── abc_dw_sl_pr_req.delta
       └── _checkpoints/

🛠️ Purpose of Split Tables

Enables better joins

Supports independent analysis of PR and PO workflows

Improves performance and filtering

🧪 Quality Checks

Null evaluations

Basic integrity checks

Column-level value checks

PR/PO deletion indicator removal

🥇 3. Gold Layer — Business KPI Layer
📌 Purpose

The Gold layer produces the final business-ready, ML-ready dataset, combining PR and PO data into one analytics table.

Table Name:
👉 abc.abc_dw_gold.abc_dw_gl_pr_po_kpi

🔗 Gold Layer Logic

Full outer join of PR and PO records

PR-only, PO-only, and matched lines

Business Day KPI calculations (Sun–Thu):

pr_approval_ageing

po_approval_ageing

pr_to_po_ageing

SLA breach flags:

sla_breach_flag

pr_cycle_sla_breach_flag

po_cycle_sla_breach_flag

Detailed vendor, material, and department attributes

Gold metadata fields:

gold_load_date

gold_load_timestamp

📄 File Structure
/gold/
   └── procurement/
       ├── abc_dw_gl_pr_po_kpi.delta
       └── _checkpoints/

📊 Purpose of Gold Layer

Central source of truth for dashboards

Used for Power BI reporting

Used for predictive ML modeling (SLA breach)

Used for supplier performance analytics

Supports executive procurement KPIs

📦 4. Data Flow Summary
SAP Extract Files (PR/PO Raw)
        ↓
  BRONZE (Raw Ingestion)
        ↓
  SILVER (Cleaned & Standardized)
        ↓
  GOLD (KPI, Business Rules, ML Ready)

📚 5. Key Technologies
Layer	Technologies Used
Bronze	ADLS, Databricks Auto Loader, Delta Lake
Silver	PySpark transformations, Date conversions, Deduplication
Gold	PySpark joins, KPI calculations, Business-day logic, SLA engines
Overall	Unity Catalog, Power BI, MLflow, GitHub
🔍 6. Quality & Governance

Follows Databricks Medallion Architecture best practices

Unity Catalog for auditing and lineage

Follows GDPR & internal data governance rules

Validated through EDA (Exploratory Data Analysis)

📁 7. Project Folder Structure
/project-root/
   ├── bronze/
   ├── silver/
   ├── gold/
   ├── notebooks/
   │     ├── eda/
   │     └── transformations/
   ├── scripts/
   ├── README.md
   └── LICENSE

📝 8. Maintainer

Taiwo Babalola
Lead Data & Analytics Specialist
ABC Logistics Limited
Email: taiwo.sap@gmail.com

GitHub: [Add link]
