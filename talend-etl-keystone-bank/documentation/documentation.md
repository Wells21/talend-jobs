# 🚀 Talend ETL Project – Keystone Bank (Client Project via Adroit Solutions)

## 🧩 Project Overview

This project involves the design and implementation of a robust, automated ETL pipeline using **Talend Open Studio** for a financial institution — **Keystone Bank**, a client of **Adroit Solutions**. The objective was to process and integrate daily flat files containing **fund transfer data** from a complex directory structure into a MySQL database.

### 🔍 Key Features:

Developed a series of **modular Talend jobs** to automatically **extract, transform, and load fund transfer flat files** from dynamically structured directories into a centralized database for Keystone Bank.

Implemented **conditional file processing logic** by segregating files into **BATCH 0\*\***, **BATCH 1\*\***, and **BATCH 2\*\*** categories, each handled by dedicated Talend jobs to **optimize error isolation and job tracking**.

Designed the jobs to handle **multiple flat files within each directory**, merging their contents before transforming and loading them into the database.

Built a specialized **"NOT MODUSER"** routine to isolate and process all flat files that do **not** originate from the `MODUSER` folder.

Orchestrated **automated job execution** using `tRunJob`, enabling a single master job to sequentially trigger all child jobs.

Included **post-load file deletion** to prevent reprocessing and ensure clean, duplicate-free ingestion.

Implemented both **T (current day)** and **T-1 (previous day)** processing logic, enabling the pipeline to recover any late or missed file drops.

Ensured **data deduplication**, **job reusability**, and **controlled execution** using Talend components like `tPrejob` and `tPostjob`.

---

## 📂 Directory & File Structure

Each flat file is stored in a structured file path like:

C:/ic4proControl/YYYYMMDD/fundstransfer/[USER]/BATCH-XXX/[FILENAME].txt


### Examples:
- `C:/ic4proControl/20250403/fundstransfer/MODUSER/BATCH-004/FT101020004.txt`
- `C:/ic4proControl/20250403/fundstransfer/USER03/BATCH-004/FT99957840004.txt`

**Folder Parameters:**
- `YYYYMMDD` – Date of transaction (used for T and T-1 logic)
- `[USER]` – Subfolder representing the user (e.g., MODUSER, PRAISE, USER10)
- `BATCH-XXX` – Classification of file groups (e.g., BATCH-0**, BATCH-1**, BATCH-2**)
- `[FILENAME].txt` – Fund transfer flat files (e.g., FT101020004.txt)

---

## 🛠 Job Design Strategy

### Modular Job Design

To improve maintainability and parallelism, the ETL logic was divided into **separate jobs**:

| Job Name | Description |
|----------|-------------|
| `BATCH-0**` Job | Extracts and processes all files from batch folders like `BATCH-001`, `BATCH-004`, etc. |
| `BATCH-1**` Job | Processes files from folders like `BATCH-124`, `BATCH-123` |
| `BATCH-2**` Job | Same logic, handles `BATCH-203`, etc. |
| `NOT MODUSER` Job | Extracts all flat files from folders **excluding** `MODUSER` |
| `Master Run Job` | Uses `tRunJob` to orchestrate execution of all sub-jobs sequentially |
| `T-1 Version` | Duplicates above jobs but targets **yesterday’s date** to handle late file drops |

---

## ⚙️ Talend Components Used

| Component | Purpose |
|----------|---------|
| `tPrejob` / `tPostjob` | Ensures controlled job start and cleanup |
| `tFileList` | Iterates through batch folders and subfolders |
| `tFileProperties` | Retrieves file names, parent paths, and timestamps |
| `tSetGlobalVar` | Captures dynamic path info for reuse |
| `tFilterRow` | Filters rows based on folder name (e.g., MODUSER) |
| `tJavaRow` | Applies custom transformation or conditional logic |
| `tFileInputDelimited` | Reads flat file content |
| `tUnite` | Combines multiple data streams for unified processing |
| `tDBOutput` | Loads transformed data into MySQL DB |
| `tDBCommit` | Ensures transaction integrity |
| `tRunJob` | Calls sub-jobs from the main orchestrator |
| `tFileDelete` | Deletes files after successful extraction to prevent duplication |

---

## 🔄 Job Execution Logic

### ✅ T-Day Jobs (Real-time)
- Processes files using current system date.
- Designed for **daily ingestion**.

### 🔁 T-1 Jobs (Backfill)
- Processes files using **yesterday’s date**.
- Captures any files dropped late or missed in previous runs.

Both sets use the same folder structure and batch filtering logic. Each job includes a **file deletion routine** to prevent reprocessing of the same files.

---

## 🧠 Sample Job Workflow

```text
tFileList → tFileProperties → tSetGlobalVar → tFilterRow → tJavaRow
                 ↓                         ↓
            (check folder)        (transform or validate)
                                       ↓
                             → tFileInputDelimited → tUnite → tDBOutput → tDBCommit
```

Files are filtered, transformed, then loaded to the database. After successful processing, they are deleted.

## 🧠 Business Impact

✅ Automated ingestion of critical fund transfer data

✅ Prevented duplication using post-processing cleanup

✅ Increased reliability through T-1 fallback logic

✅ Enhanced scalability and modular job control

✅ Delivered maintainable, production-grade Talend workflows

