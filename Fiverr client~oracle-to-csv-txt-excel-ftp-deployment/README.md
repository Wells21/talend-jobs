# End-to-End ETL Workflow: Oracle to CSV, TXT & Excel with FTP Deployment (with Terminated Employees Logic)

This project implements a fully automated **ETL** pipeline using **Talend Cloud Data Fabric**.  
The workflow extracts employee data from an **Oracle Database**, applies transformations to ensure data consistency, and outputs the results into multiple formats (**CSV, TXT, Excel**). Finally, the pipeline performs conditional validations before securely deploying the files to a **GoAnywhere FTP server**.

In addition, the workflow includes logic for detecting **terminated employees** by comparing the current run data with the previous run data. Terminated employees are not only exported into files and FTP’d but are also saved into a dedicated **database table** for long-term querying and reporting.

---

## 🚀 Project Overview

- **Source System**: Oracle Database  
- **ETL Tool**: Talend Cloud Data Fabric  
- **Output Targets**: CSV, TXT, Excel  
- **Deployment Target**: GoAnywhere FTP Server  
- **Database Tables**: `previous_run`, `terminated`  
- **Version Control**: Git integrated for job migration (Test → Production)  

---

## 🎯 Objectives

1. Extract employee data from an Oracle database.  
2. Transform the **workphoneno** field for consistent formatting.  
3. Generate multiple file outputs (CSV, TXT, Excel).  
4. Validate files to ensure they are not empty before deployment.  
5. Securely upload only valid files to a remote FTP server.  
6. Detect **terminated employees** by comparing previous vs. current run.  
7. Store terminated employees into a dedicated **database table** for query/reporting.  
8. Automate updates of the `previous_run` table after each job execution.  

---

## 🛠 Key Components & Workflow

1. **Dynamic Path Management**  
   - `tPrejob + tJava`: Generate global variables with timestamps for unique file names.  

2. **Data Extraction & Transformation**  
   - `tDBInput`: Extract employee data.  
   - `tMap`: Format `workphoneno`.  

3. **File Generation**  
   - `tReplicate`: Branch data into CSV, TXT, Excel outputs.  

4. **Validation Checks & FTP Deployment**  
   - `tFileRowCount` (CSV/TXT), `tAggregateRow + tJavaRow` (Excel).  
   - Conditional FTP upload only if rows > 0.  

5. **Terminated Employees Logic**  
   - Compare **previous_run** vs **current run** in `tMap` (left join).  
   - Extract records not present in current run (terminated employees).  
   - Save results into:  
     - `terminated` DB table (for management queries).  
     - CSV, TXT, Excel file outputs.  
   - Validate and FTP only if rows > 0.  

6. **Post-Job Update**  
   - `tDBOutput`: Refresh `previous_run` table with current run data after each execution.  

---

## ✅ Results

- **Data Reliability**: Ensures only valid data files are deployed.  
- **Automation**: Handles both active and terminated employee workflows.  
- **Business Value**: Provides historical visibility into terminated employees for management.  
- **Scalability**: Can easily be extended to other HR workflows.  
- **Reusability**: Modular job design ensures maintainability.  
