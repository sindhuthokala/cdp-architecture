# 📥 Customer360 Data Ingestion Flow

This document explains how raw data from all enterprise systems is collected, validated, and stored inside the Customer 360 platform. It defines ingestion patterns, validation rules, quality checks, and the final outputs that feed downstream layers such as Identity Resolution and Customer Profile modeling.

---

## 🎯 Objectives
- Collect data reliably from all customer-touching systems  
- Support batch, streaming, and API-based ingestion  
- Apply schema & quality validation before downstream processing  
- Maintain an auditable, immutable Raw Layer for all incoming data  
- Prepare data for identity stitching and unified profile creation  

---

## 🗂️ Source Systems
Customer data may originate from:

- **CRM & Loyalty Systems** (customer profiles)  
- **Web & Mobile Apps** (events, sessions, device IDs)  
- **Marketing Platforms** (email events, campaign metadata)  
- **POS / Ecommerce Systems** (transactions, carts, product data)  
- **Support Tools** (tickets, chat logs, voice transcripts)  
- **3rd-party Enrichment Providers** (demographic or behavioral data)  

---

## 🏗️ Extraction Methods

### **1. Batch Exports**
- Scheduled CSV/Parquet drops (hourly, daily, weekly)  
- Often used by CRM, POS, legacy systems  
- Stored temporarily before ingestion  

### **2. Streaming Events**
- Real-time web/app events (page views, checkouts, logins)  
- Delivered through event buses or streaming gateways  
- Ideal for personalization and behavioral analytics  

### **3. API Connectors**
- Pull or push integration with marketing or CRM platforms  
- Used for incremental sync of customer updates  

### **4. Manual / Automated File Upload**
- SFTP, cloud bucket upload, or vendor exports  

---

## 🧹 Ingestion Layer Processing

### **Schema Validation**
- Required fields (e.g., `email`, `customer_id`)  
- Data types (e.g., timestamps, integers, arrays)  
- Allowed enum values  

### **Field-Level Quality Checks**
- Proper email formatting  
- Location data validation  
- Device ID checksum checks  

### **Duplicate Detection**
- Remove exact duplicates  
- Flag near-duplicates for downstream identity logic  

### **Quarantine Handling**
- Invalid records are stored in a separate error bucket  
- Full lineage maintained for debugging  

---

## 🗄️ Raw Layer Storage
After validation, data is deposited into the Raw Layer:

- **Immutable** — never modified  
- **Partitioned** by date/source for efficient querying  
- **Schema-preserving** — stores original structure  
- **Time-stamped** for auditing and reproducibility  

---

## 🔎 ASCII Architecture Diagram
[ Source Systems ]
|
v
[ Extraction Layer ]
batch / api / streaming
|
v
[ Ingestion Layer ]
validation + normalization
|
v
[ Raw Storage Layer ]

---

## ✅ Outputs of the Ingestion Flow
- Clean, structured, validated customer data  
- Organized raw datasets ready for transformation  
- Event streams prepared for Identity Resolution  
