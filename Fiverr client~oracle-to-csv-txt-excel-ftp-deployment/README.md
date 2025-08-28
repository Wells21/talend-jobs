# End-to-End ETL Workflow: Oracle to CSV, TXT & Excel with FTP Deployment

This project implements a fully automated **ETL** pipeline using **Talend Cloud Data Fabric**.  
The workflow extracts data from an **Oracle Database**, applies transformations to ensure data consistency, and outputs the results into multiple formats (**CSV, TXT, Excel**). Finally, the pipeline performs conditional validations before securely deploying the files to a **GoAnywhere FTP server**.

---

## 🚀 Project Overview

- **Source System**: Oracle Database  
- **ETL Tool**: Talend Cloud Data Fabric  
- **Output Targets**: CSV, TXT, Excel  
- **Deployment Target**: GoAnywhere FTP Server  
- **Version Control**: Git integrated for job migration (Test → Production)  

---

## 🎯 Objectives

1. Extract employee data from an Oracle database.  
2. Transform the **workphoneno** field to maintain consistent formatting.  
3. Generate multiple file outputs (CSV, TXT, Excel).  
4. Validate files to ensure they are not empty before deployment.  
5. Securely upload only valid files to a remote FTP server.  
6. Implement logging to track job execution in the cloud environment.  

---

## 🛠 Key Components & Workflow

1. **Dynamic Path Management**  
   - `tPrejob + tJava`: Dynamically generate global variables with timestamps to prevent duplicate file issues.  
   - Environment-aware paths configured for smooth transition between **local/test** and **cloud/production**.  

2. **Data Extraction & Transformation**  
   - `tDBConnection + tDBInput`: Extract data from Oracle.  
   - `tMap`: Transform `workphoneno` field with custom Java expressions.  

3. **File Generation**  
   - `tReplicate`: Branch data into multiple outputs.  
   - `tFileOutputDelimited`: Write CSV and TXT.  
   - `tFileOutputExcel`: Write Excel files.  

4. **Validation Checks**  
   - `tFileRowCount`: Validate CSV and TXT.  
   - `tAggregateRow + tJavaRow`: Validate Excel output.  
   - Conditional flow ensures **only non-empty files** are sent to FTP.  

5. **FTP Deployment**  
   - `tFTPPut`: Securely upload validated files to **GoAnywhere FTP**.  

6. **Logging & Monitoring**  
   - `tLogRow + tJavaRow`: Custom logging for execution flow and errors.  
   - Critical for **cloud deployment**, since visual job progress is not visible in Talend Cloud.  

---

## ✅ Results

- **Data Reliability**: Ensures only valid data files are deployed.  
- **Automation**: Eliminates manual validation and file handling.  
- **Scalability**: Easily extendable to new file formats or data sources.  
- **Reusability**: Modular job design makes it adaptable to future workflows.  

---

## 📂 Repository Structure


