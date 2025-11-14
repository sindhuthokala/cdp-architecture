# Customer360 – High-Level Data Flow (Phase 1)

This document describes the high-level data flow for the **Customer360 Intelligent Insights Platform – Phase 1**.

It connects directly to the goals in the Customer360 PRD:
- Ingest data from multiple systems
- Create a unified customer profile
- Enable analytics and segmentation

---

## 1. Source Systems

Phase 1 focuses on a small but realistic set of core systems:

- **CRM** – customer records, basic attributes, lifecycle status  
- **Web/App Events** – page views, clicks, sessions, key events  
- **Transactions** – orders, revenue, product details  

These systems are the inputs into the Customer360 foundation.

---

## 2. High-Level Flow

At a high level, data moves like this:

```text
Source Systems                Data Platform / Warehouse         Customer360 Layer          Analytics & Activation
----------------------       -----------------------------     ----------------------     -------------------------
CRM  -------------------->   Ingestion & Staging  ---------\
Web Events  -------------->   (e.g., Snowflake / BigQuery)   ---> Unified Profiles  --->   BI Dashboards & Segments
Transactions  ------------>                                /       & Events          \
                                                           \                          --->  Downstream Tools (future)
                                                            ----------------------

---

## 3. Steps in the Flow

Below is the step-by-step breakdown of how data moves through the Customer360 system in Phase 1.

### **Step 1: Extraction**
Data is extracted from the source systems using:
- Batch exports (CSV, Parquet)
- REST APIs
- Event tracking SDKs (for web/app)

**Key idea:** Raw data leaves each system in its original form.

---

### **Step 2: Landing & Staging**
Raw data is loaded into a **staging area** inside the data platform (Snowflake / BigQuery).

This layer handles:
- Initial file loading  
- Basic schema checks  
- Minimal validation (nulls, types)

**Why staging exists:**  
It protects downstream logic from dirty or inconsistent raw data.

---

### **Step 3: Transformation & Standardization**
Data from each source is cleaned, normalized, and mapped to a shared structure.

Common transformations:
- Deduplication  
- Normalizing country/region  
- Standardizing event names  
- Converting timestamps to UTC  
- Mapping to Customer360 core fields  

**Output:** Cleaned tables ready for identity stitching.

---

### **Step 4: Identity Resolution**
The system links all records that belong to the same person.

**Identifiers used:**
- CRM → `customer_id`, `email`  
- Web → `device_id`, `cookie_id`, `user_id` (if logged in)  
- Transactions → `customer_id`, `order_id`

**Basic business rules:**
- Same email → same customer  
- Same customer_id across systems → unify  
- Logged-in web events override anonymous events  

**Outcome:**  
A single **Customer360_ID** per individual.

---

### **Step 5: Unified Profiles & Events Layer**
Two core tables are created:

#### `customer360_profiles`
- 1 row per customer  
- Lifecycle attributes  
- Engagement metrics  
- CRM + transaction + behavior fields combined  

#### `customer360_events`
- All behavioral data (web, transactions, actions)  
- Timestamped  
- Ready for BI or ML consumption  

**Purpose:**  
These tables power analytics, segmentation, and future AI.

---

### **Step 6: Consumption Layer**
Data is now available for downstream tools:

#### ✔ Analytics (Phase 1)
- Power BI / Looker dashboards  
- Retention analysis  
- Churn signals  
- Customer lifecycle insights  

#### ✘ Activation (Future Phase)
- Ad platforms  
- Email systems  
- Journey orchestration  

(The architecture supports this but does not deliver it yet.)

---

