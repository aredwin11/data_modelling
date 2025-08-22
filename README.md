
# Data Modelling using Netflix Dataset

## **Overview**
This project implements a **modern data lakehouse architecture** using the **Bronze, Silver, and Gold layers** approach to transform raw Netflix dataset into business-ready analytics tables. The pipeline follows best practices like data cleansing, standardization, surrogate key generation, and dimensional modeling.



## **Architecture Layers**

### **1. Bronze Layer – Raw Data Ingestion**
- Ingests raw Netflix data (CSV/JSON) into the Bronze layer.
- Stores unprocessed data for audit and replay purposes.
- Preserves the original schema and format.

### **2. Silver Layer – Data Cleaning & Transformation**
- Read Input Table: Load `bronze_source` table from bronze schema.
- Remove Nulls: Dropped all rows where any column had NULL values.
- Explode Multi-Valued Country Field**: Split and exploded comma-separated values in the `country` column into separate rows.
- Duration Split: 
   - Split `duration` into `duration_minutes` and `duration_seasons`.
  - Dropped the original `duration` column.
- Clean Dummy Date Values: Replaced invalid entries in `date_added` with `Unknown`.
- Handle Empty Country Values: Replaced missing/empty values in `country` with `Unknown`.
- Standardize Rating Column: Replaced invalid minute-based entries in `rating` with `Not Rated`.
- Save Transformed Data: Stored the cleaned data as `silver_source` in the silver schema.

### **3. Gold Layer – Dimensional & Fact Modeling**
Designed a **star schema** for analytical queries:
- **Dimension Tables:**
  - `dim_show`: Details of shows (title, type, description).
  - `dim_director`: List of directors.
  - `dim_cast`: Actor details.
  - `dim_country`: Country details.
  - `dim_date`: Date dimension derived from `date_added` (year, month, day).
  - `dim_category`: Genre/category details (from `listed_in`).
  - `dim_rating`: Rating information.
- **Fact Table:**
  - Links all dimensions with surrogate keys.
  - Stores measures and references for analytical reporting.

Implemented:
- **Surrogate keys**: Auto-generated numeric IDs for each dimension.
- **Primary keys**: Unique identifiers in each dimension.
- **Foreign keys**: References from fact table to dimension tables.



## **Outcome**
This pipeline delivers **clean, normalized, and analytics-ready Netflix data**, enabling efficient reporting and business intelligence.
