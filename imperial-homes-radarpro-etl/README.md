# 🏦 Talend ETL: Imperial Homes Mortgage Bank → RadarPro Integration

This project automates the extraction and transformation of financial, personal, and transactional data from **Imperial Homes Mortgage Bank’s Core Banking Application (CBA)** to Adroit's **RadarPro** software using **Talend Open Studio**.

The solution was designed to support **daily regulatory reporting**, specifically **CTR (Currency Transaction Reports)** required by the NFIU.

---

## 📌 Key Features

- 🔁 Extracts data from the bank’s CBA system into a destination Microsoft SQL Server database (RadarPro)
- 🧠 SQL-driven transformation logic to map source fields to RadarPro schema
- 🧩 Modular jobs for:
  - Customer Data
  - Customer Account Data
  - Customer Identity
  - Occupation
  - Address
  - Transactional Data
- ⚙️ Automates daily ingestion via **Windows Task Scheduler**
- ✅ Ensures clean, error-free extraction with `tPrejob`, `tDBCommit`, and controlled connections

---

## 🛠 Components Used

- `tDBInput` – Extracts data from the source banking database
- `tLogRow` – Used during development to validate transformations
- `tDBOutput` – Loads cleaned data into the RadarPro database
- `tDBCommit` – Ensures transactional integrity
- `tPrejob` – Initializes connections and sets pre-conditions
- `tDBConnection` – Manages database sessions

---

## 🧪 Example Job Flow

```text
tPrejob
   ↓
tDBConnection → tDBInput (SQL transformation) → tDBOutput → tDBCommit
```

## 🧠 Business Impact
✅ Supported compliance reporting (CTR) by enabling timely, structured data delivery

⚡ Improved performance for large datasets (millions of rows) using direct SQL logic

⏱ Automated ETL schedule ensured real-time data sync with minimal human input

🧾 Enabled seamless ingestion of customer and transaction data into RadarPro

🎯 Delivered zero-error, production-grade pipelines integrated into a live banking environment

