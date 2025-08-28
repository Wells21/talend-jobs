# Project Documentation: End-to-End ETL Workflow

## 1. Introduction

This document provides a detailed technical walkthrough of the **End-to-End ETL Workflow** developed in **Talend Cloud Data Fabric**.  
The goal was to automate the extraction, transformation, validation, and deployment of employee data from an Oracle database into multiple file formats with secure FTP transfer.

---

## 2. Project Scope

- **Data Source**: Oracle database (Employee records).  
- **Data Transformation**: Format standardization of the `workphoneno` field.  
- **Outputs**: CSV, TXT, Excel.  
- **Deployment**: Remote FTP server (GoAnywhere).  
- **Environment Migration**: Test → Production via Git integration.  
- **Logging**: Implemented for cloud monitoring.  

---

## 3. Architecture

### High-Level Flow:
1. **Extract** data from Oracle.  
2. **Transform** fields for consistency.  
3. **Replicate** and write to CSV, TXT, Excel.  
4. **Validate** file contents.  
5. **Upload** only valid files to FTP.  
6. **Log** every step for traceability.  

---

## 4. Detailed Component Workflow

### 4.1 Pre-Job Setup
- **tPrejob + tJava**  
  - Define global variables for file paths.  
  - Append timestamps to filenames to avoid overwrites.  
  - Handle dynamic path changes across environments (local/test vs. cloud/prod).  

### 4.2 Database Connection
- **tDBConnection + tDBInput**  
  - Establish Oracle DB connection.  
  - Retrieve employee data using parameterized queries.  

### 4.3 Transformation
- **tMap**  
  - Normalize `workphoneno` with custom Java expressions.  
  - Map fields into structured output schema.  

### 4.4 File Generation
- **tReplicate**: Split dataset for parallel outputs.  
- **tFileOutputDelimited (CSV, TXT)**  
- **tFileOutputExcel (Excel)**  

### 4.5 File Validation
- **CSV & TXT**: `tFileRowCount` ensures row count > 0.  
- **Excel**: `tAggregateRow + tJavaRow` ensures non-empty content.  
- **tRunIf conditions**: Prevents empty files from reaching FTP stage.  

### 4.6 FTP Deployment
- **tFTPPut**  
  - Transfers validated files to **GoAnywhere FTP server**.  
  - Configured for secure credentials and remote directory structure.  

### 4.7 Logging
- **tLogRow + tJavaRow**  
  - Custom execution logs for each stage.  
  - Logs include:  
    - Start & completion time  
    - Number of records processed  
    - File path references  
    - FTP success/failure status  
  - Necessary for cloud jobs where live job visualization is not available.  

---

## 5. Git Integration & Cloud Deployment

- Talend job versioned using **Git**.  
- Migration path: **Local Development → Test → Production**.  
- Environment variables dynamically adjusted to handle cloud paths.  
- Jobs pushed to **Talend Cloud Management Console (TMC)** for execution and scheduling.  

---

## 6. Challenges & Solutions

1. **Environment-specific paths**  
   - Solution: Used global variables and tJava for dynamic path management.  

2. **Cloud job monitoring limitations**  
   - Solution: Implemented detailed logging with tLogRow and tJavaRow.  

3. **File validation across formats**  
   - Solution: Used different validation techniques per file type (row count, aggregation).  

---

## 7. Results

- Robust and reusable ETL workflow.  
- Automated end-to-end process with zero manual intervention.  
- Data quality improved via transformations and validations.  
- Logs ensured transparency in cloud execution.  
- Secure and reliable FTP deployment.  

---


