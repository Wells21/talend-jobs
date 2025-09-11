# Project Documentation: End-to-End ETL Workflow with Terminated Employees Logic

## 1. Introduction

This document provides a detailed walkthrough of the **ETL Workflow** developed in **Talend Cloud Data Fabric**.  
The solution automates the extraction, transformation, validation, and deployment of employee data, while also tracking **terminated employees** by comparing consecutive job runs.

---

## 2. Project Scope

- **Data Source**: Oracle database (Employee records).  
- **Data Transformation**: Formatting of the `workphoneno` field.  
- **Outputs**: CSV, TXT, Excel.  
- **Deployment**: Remote FTP server (GoAnywhere).  
- **Additional Logic**:  
  - Terminated employees detection.  
  - Historical storage of terminated employees in a dedicated DB table.  
  - Updating of a `previous_run` table after every execution.  
- **Logging**: Implemented for cloud monitoring.  

---

## 3. Architecture

### High-Level Flow:
1. **Extract** current run data from Oracle.  
2. **Transform** fields for consistency.  
3. **Replicate** and write to CSV, TXT, Excel.  
4. **Validate** file contents.  
5. **Upload** only non-empty files to FTP.  
6. **Compare** current vs previous run to detect terminated employees.  
7. **Store** terminated employees in DB and export to files.  
8. **Update** previous_run table.  
9. **Log** all steps for traceability.  

---

## 4. Detailed Component Workflow

### 4.1 Pre-Job Setup
- `tPrejob + tJava`:  
  - Dynamic global variables for file paths with timestamps.  
  - Avoid duplicate overwrites.  

### 4.2 Active Employees Flow
- `tDBInput`: Extract employee records.  
- `tMap`: Transform `workphoneno`.  
- `tReplicate`: Output to multiple file types.  
- `tFileRowCount / tAggregateRow`: Validate row counts.  
- `tFTPPut`: Upload validated files to FTP.  

### 4.3 Terminated Employees Flow
- **Comparison**:  
  - `tDBInput` (previous_run table) as main.  
  - `tDBInput` (current run table) as lookup.  
  - `tMap`: Left join on primary key.  
  - Filter rows where current PK is `null` → terminated employees.  
- **Storage**:  
  - `tDBOutput`: Insert terminated employees into a dedicated DB table `terminated`.  
- **Export & Deployment**:  
  - Save into CSV, TXT, Excel.  
  - Row validation before FTP upload.  

### 4.4 Post-Job Update
- `tPostJob + tDBOutput`:  
  - Truncate `previous_run`.  
  - Insert current run data as new baseline.  

### 4.5 Logging
- `tLogRow + tJavaRow`: Execution logs with:  
  - Record counts.  
  - File paths.  
  - FTP transfer results.  
  - Job duration and errors.  

---

## 5. Git Integration & Cloud Deployment

- Git version control for job promotion.  
- Migration path: **Local → Test → Production**.  
- Cloud deployment in **Talend Management Console (TMC)** with scheduling enabled.  

---

## 6. Challenges & Solutions

1. **Detecting terminated employees**  
   - Solution: Left join on PK between previous_run and current run.  

2. **Maintaining previous run data**  
   - Solution: Post-job update ensures `previous_run` is always refreshed.  

3. **Cloud monitoring limitations**  
   - Solution: Detailed logs via `tLogRow` and `tJavaRow`.  

---

## 7. Results

- Fully automated ETL job handling **both active and terminated employees**.  
- Enhanced HR analytics by persisting terminated employees in DB.  
- Zero manual intervention required for validation or FTP transfers.  
- Improved reliability, scalability, and auditability.  
