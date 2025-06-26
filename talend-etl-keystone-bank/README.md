# Talend ETL Pipeline – Keystone Bank (Client Project)

This repository contains the Talend ETL jobs I built to automate the ingestion of daily fund transfer flat files from Keystone Bank’s directory structure into a MySQL database.

## 📂 Project Structure
- 🔄 Extracts flat files from nested batch folders (e.g., `BATCH-0**`, `BATCH-1**`)
- 🔍 Filters based on user (e.g., `MODUSER`, `NOT MODUSER`)
- 🔧 Transforms and loads into a structured database table
- 🧹 Deletes processed files to avoid duplication
- 🕐 Includes jobs for T (today) and T-1 (yesterday) processing

## 🚀 Talend Components Used
`tFileList`, `tFileProperties`, `tMap`, `tRunJob`, `tDBOutput`, `tFileDelete`, `tPrejob`, `tPostjob`, `tFilterRow`, `tJavaRow`, `tUnite`, `tSetGlobalVar`, 
`tDBCommit`

## 📸 Screenshots
See `/screenshots` folder for detailed job flows.

## 📁 Job Files
Jobs exported as `.zip` can be imported directly into Talend Open Studio.

## 📌 Sample Path Pattern
C:/ic4proControl/YYYYMMDD/fundstransfer/MODUSER/BATCH-004/FT101020004.txt
