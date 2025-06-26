# 🏦 ETL Integration – Imperial Homes Mortgage Bank → RadarPro

## 📘 Project Overview

This project involved building a robust, automated ETL pipeline for **Imperial Homes Mortgage Bank**, a client of **Adroit Solutions**. The goal was to extract, transform, and load multiple data domains from the bank’s **Core Banking Application (CBA)** into **RadarPro**, an in-house compliance software developed by Adroit.

The extracted data is used primarily to support regulatory reporting — most notably the **Currency Transaction Report (CTR)**, which is required by the **Nigerian Financial Intelligence Unit (NFIU)**.

---

## 🧩 Scope & Components

### 📌 Extracted Data Domains
- Customer personal details
- Customer account data
- Customer identity information
- Customer occupation
- Customer address
- Customer transaction history

Each of these domains is handled by **dedicated Talend jobs** to promote modularity and reduce failure impact.

---

## ⚙️ ETL Process Design

Each Talend job follows a similar design pattern:

```text
tPrejob 
   ↓
tDBConnection (Source DB)
   ↓
tDBInput (SQL-based transformation)
   ↓
tDBOutput (Destination: RadarPro DB)
   ↓
tDBCommit
```

SQL-based transformations are applied directly in tDBInput queries to reshape and rename columns based on RadarPro schema requirements.

Transactional integrity is enforced using tDBCommit.

Job orchestration is handled through Windows Task Scheduler to ensure daily automated runs.

## 🔧 Talend Components Used

| Component       | Purpose                                                      |
| --------------- | ------------------------------------------------------------ |
| `tPrejob`       | Initializes preconditions and metadata loading               |
| `tDBConnection` | Connects to both source (CBA) and destination (RadarPro) DBs |
| `tDBInput`      | Executes SQL queries with transformation logic               |
| `tDBOutput`     | Inserts transformed data into RadarPro tables                |
| `tDBCommit`     | Ensures commit after data is written                         |
| `tLogRow`       | (Dev only) Used for inspecting row values in testing         |

## ⏱ Job Automation

ETL jobs were scheduled to run daily via Windows Task Scheduler, ensuring:

Real-time ingestion of new customer and transaction data

No manual intervention required

Consistent refresh of RadarPro reporting layer

## 🛠 SQL Transformation Logic

The transformation from CBA schema to RadarPro fields was handled using custom SQL inside tDBInput.

Example:
```
SELECT  
CONCAT (B.TransID, B.SeqNo) AS RICATRANSREFID,
B.[TransID] AS RICATRANSID,
B.branchAdded AS RICABRANCHCODE,
B.BranchName AS RICABRANCHNAME,
B.AccountNo AS RICAACCOUNTID...

```

Such mappings were implemented across all jobs to maintain schema integrity and compliance standards.

## 📈 Business Outcomes

✅ Regulatory Compliance
Automated generation of CTR reports for NFIU submission.

⚡ Real-Time Ingestion
Daily job scheduling ensured that no recent transactions were missed.

📦 Scalable Solution
Designed to handle large data volumes — millions of rows per extract.

🎯 Error-Free Delivery
Modular jobs + DB commits = high reliability and clean termination.

🛠 Low Maintenance
SQL-based transformations minimized Talend component complexity.

## 🧪 Testing & Validation

- Manual run-throughs with tLogRow to inspect test outputs
- Monitored job success/failure logs via Task Scheduler
- Spot validation on destination tables using SQL scripts


