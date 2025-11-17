# Customer360 Ingestion Data Flow

## Overview
This document describes how customer data is ingested from multiple source systems into the Customer360 ecosystem. The ingestion pipeline standardizes, validates, and delivers data into the processing/storage layers for downstream use.

---

## Objectives
- Collect customer data from all systems reliably  
- Support batch, streaming, and API ingestion  
- Normalize formats across sources  
- Ensure data quality and schema consistency  
- Enable near–real-time availability where needed  

---

## High-Level Steps

### 1. Data Extraction
Data enters the system from:
- CRM exports  
- Website & mobile tracking events  
- Marketing platform engagement logs  
- POS transactions  
- Customer support system exports  
- Vendor feeds / Partner files  

Extraction types:
- **Batch**: nightly/hourly dumps  
- **Streaming**: real-time events  
- **API**: REST/GraphQL pulls  
- **File uploads**: CSV, JSON, parquet  

---

### 2. Ingestion Layer
Once extracted, data flows through:

#### **a. Batch Pipelines**
- Scheduled ETL/ELT jobs  
- Bulk loads to Raw Layer  
- Validation and schema mapping  

#### **b. Streaming Pipelines**
- Real-time events from SDKs or tag manager  
- Event queues / pub-sub topics  
- JSON payloads normalized into raw logs  

#### **c. API Connectors**
- CRM sync APIs  
- Mobile app identity APIs  
- Third-party marketing APIs  

---

### 3. Data Validation
Performed automatically during ingestion:

- Schema checks  
- Field-level validation  
- Required field enforcement  
- Duplicate detection  
- PII masking (if applicable)  

Invalid records → quarantine for review.

---

### 4. Deposit into Raw Layer
All ingested data (valid or invalid) is stored in the **Raw Layer**, maintaining source system fidelity.

Characteristics:
- Immutable  
- Time-stamped  
- Partitioned  
- Auditable  

---

### Data Flow Summary (ASCII Diagram)

