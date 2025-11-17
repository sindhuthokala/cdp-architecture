# 🧱 Customer360 Data Modeling

This document describes the structured data models used to transform raw data into curated Customer360 tables. It defines schemas, layers (Bronze/Silver/Gold), and entity relationships.

---

## 🎯 Objectives
- Create standardized schemas for customer and behavioral data  
- Normalize and unify attributes across systems  
- Support analytics, activation, and ML use cases  
- Maintain high data quality and lineage  

---

# 🏗️ Data Model Layers

### **1. Bronze Layer (Cleaned Raw)**
- Minimal transformations  
- Standardized field names  
- Quality checks applied  
- Still close to source-level structures  

### **2. Silver Layer (Normalized Models)**
- Deduplicated  
- Conformed dimensions  
- Unified identifiers (Customer360_ID)  
- Standardized event & entity models  

### **3. Gold Layer (Business/Purpose Models)**
- Ready for analytics dashboards  
- Segment tables  
- ML-ready datasets  
- Customer 360 Profile tables  

---

# 👤 Customer Profile Schema (Gold Layer)

| Field | Description |
|-------|-------------|
| customer360_id | Master identity key |
| first_name | Unified first name |
| last_name | Unified last name |
| primary_email | Winning/merged email |
| phones | Array of phone numbers |
| address | Unified postal address |
| demographics | Age, gender, income (if available) |
| loyalty_info | Points, tier, join date |
| last_seen | Most recent event timestamp |
| lifetime_value | Aggregated CLV metric |
| churn_risk | ML-derived score |

---

# ⚡ Event Schema Example
A single standardized event format:

```json
{
  "event_name": "product_viewed",
  "event_timestamp": "2024-01-01T12:45:32Z",
  "customer360_id": "abc123",
  "device_id": "xyz789",
  "page_url": "/products/1234",
  "product_id": "SKU-987",
  "metadata": {
    "referrer": "google",
    "campaign": "spring_sale"
  }
}

