# Snowflake to Salesforce Integration Pipeline (Talend)

## 📌 Project Overview

This project implements an end-to-end ETL pipeline using **Talend** to:

1. Extract structured data from a **Snowflake** data warehouse  
2. Transform and align the schema to match **Salesforce API object fields**  
3. Load the transformed data into **Salesforce** using the Bulk API  

The solution is designed for high-volume ingestion, schema consistency, error handling, and production stability.

---

## 🏗️ Architecture Overview

**Source** → Snowflake  
**Transformation Layer** → Talend (tDBInput, tMap, validation, logging)  
**Target** → Salesforce (Bulk API)

### High-Level Flow

Snowflake → Talend Extraction → Schema Mapping (tMap)
→ Salesforce Bulk Output → Bulk Execution
→ Success / Reject Handling → Logging


---

## ⚙️ Technologies Used

- Talend Open Studio / Talend Data Integration  
- Snowflake  
- Salesforce Bulk API  
- SQL  
- API-based Data Integration  
- Logging & Reject Handling Strategy  

---

## 🔄 Workflow Breakdown

### 1️⃣ Pre-Job Initialization

Talend initializes required connections and security configurations.

**Components Used:**

- `tPrejob`  
- `tSetTalendKeyStore`  
- Snowflake connection component  
- `tSalesforceConnection`  

**Responsibilities:**

- Initialize context variables  
- Establish secure Snowflake connection  
- Establish Salesforce API session  
- Ensure job execution order control  

---

### 2️⃣ Data Extraction (Snowflake)

**Component:** `tDBInput`

**Responsibilities:**

- Execute SQL query against Snowflake  
- Retrieve required dataset  
- Enforce explicit column selection  
- Apply optional filtering  

**Example Query (Conceptual):**

```sql
SELECT 
    building_id,
    building_name,
    city,
    state,
    created_date
FROM analytics.buildings;
```

3️⃣ Data Transformation & Field Mapping

Component: tMap

This layer aligns the Snowflake schema with the Salesforce object structure.

Responsibilities:

1. Mapped Snowflake fields to Salesforce object fields

2. Performed data type alignment

3. Handled null values

4. Applied conditional logic

5. Formatted date/time fields

6. Applied default values where required

7. Applied Picklist logic on some field, making sure whatever data coming in must map to 5 pickilist vlaues if not reject.
This ensures data quality on the salesforce object.

Example Field Mapping;

| Snowflake Column | Salesforce Field |
| ---------------- | ---------------- |
| building_id      | External_Id__c   |
| building_name    | Name             |
| city             | mayestro__City__c          |
| state            | mayestro__State__c         |
| created_date     | mayestro__Created_Date__c  |

This explicit mapping prevents schema drift and runtime failures.

4️⃣ Bulk Data Load (Salesforce)

Components Used:

* tSalesforceOutputBulk

* tSalesforceBulkExec

Load Strategy:

* Bulk API mode

* Batch processing enabled

* Insert / Upsert based on use case

* External ID-based deduplication

Why Bulk API?

* Optimized for high-volume records

* Asynchronous processing

* Reduced API call overhead

* Better performance vs. REST mode

5️⃣ Error Handling & Logging

The job implements structured error handling to prevent silent failures.

Mechanisms Used:

* Reject flow (Reject link)

* tLogRow for debugging (removed during production)

* Optional file output for failed records

Flow Structure:

* Main Flow → Successfully processed records

* Reject Flow → Failed records captured separately

This ensures traceability and resilience.


📈 Business Impact

Automated manual data synchronization

Reduced integration errors

Improved data consistency between Snowflake and Salesforce

Scalable architecture for future growth

Production-ready ETL framework


👤 Author

Wellspring
Data Engineer | Talend ETL Developer | SQL & API Integration Specialist
