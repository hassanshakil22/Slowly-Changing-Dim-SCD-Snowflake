# End-to-End Data Pipeline with Slowly Changing Dimensions (SCD)

This project demonstrates a fully automated **data engineering pipeline** that implements **Slowly Changing Dimensions (SCD Type 1 & 2)** using **Apache NiFi**, **AWS S3**, and **Snowflake**.  
It automates data generation, ingestion, and historical change tracking from source to warehouse — all without manual intervention.

---

## 🧠 Project Overview

The goal of this project was to design and deploy a data pipeline that:
- Generates synthetic customer data using **Python (Faker)**.
- Automates ETL flow using **Apache NiFi** hosted on **AWS EC2**.
- Loads data into **Snowflake** using external stages and **auto-ingesting Snowpipe**.
- Implements **SCD Type 1** (overwrite changes) and **SCD Type 2** (track full history).

---

## ⚙️ Architecture Workflow

**1. Data Generation (Source Layer)**
- Created synthetic customer data in Jupyter Notebook using the `Faker` library.
- Exported as `customer_YYYYMMDDHHMMSS.csv`.

**2. Data Movement (NiFi Layer)**
- Hosted **Apache NiFi** on an AWS EC2 instance.
- Built a NiFi flow:  
  `ListFile → FetchFile → PutS3Object`
- NiFi automatically pushes every new CSV file into an **Amazon S3 bucket**.

**3. Storage & Ingestion (AWS → Snowflake Layer)**
- Configured **S3 bucket** as an **external stage** in Snowflake.
- Created a **Snowpipe** to auto-ingest new files uploaded to S3.
- New data lands in the **customer_raw** table in real-time.

**4. Data Warehousing (SCD Logic in Snowflake)**
- Implemented **SCD Type 1** to handle direct updates (overwrite changes).
- Implemented **SCD Type 2** with:
  - `start_time`, `end_time`, and `is_current` flags.
  - `INSERT`, `UPDATE`, `DELETE` DML logic.
  - Stream & Task automation for incremental tracking.

**5. Automation**
- The entire process — from data generation to historical tracking — runs **without manual triggers**.
- Each layer reacts to the next through event-driven or scheduled automation.

---

## 🧩 Tech Stack

| Component | Technology Used |
|------------|----------------|
| **Data Generation** | Python (Faker Library, Jupyter Notebook) |
| **ETL Orchestration** | Apache NiFi (on AWS EC2) |
| **Cloud Storage** | Amazon S3 |
| **Data Warehouse** | Snowflake |
| **Automation** | Snowpipe, Streams, and Tasks |
| **Language/Tools** | SQL, Python, Bash |

---

## 📊 Example Flow

