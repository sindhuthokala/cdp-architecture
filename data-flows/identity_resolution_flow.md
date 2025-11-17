# 🧩 Customer360 Identity Resolution Flow

Identity Resolution determines whether records from multiple systems belong to the same individual. This document describes matching rules, merge logic, survivorship policies, and the structure of the identity graph.

---

## 🎯 Objectives
- Consolidate fragmented customer records  
- Create a persistent, unified identity across systems  
- Support deterministic and probabilistic matching  
- Maintain a Customer Graph to track relationships  
- Power 360° profile generation and personalization  

---

## 🧩 Matching Types

### **1. Deterministic Matching (Exact Match)**
Strict, rules-based matches:
- Email address  
- Phone number  
- Loyalty ID  
- Device ID  
- Login/Account ID  

Highly accurate, low recall.

---

### **2. Probabilistic Matching (Fuzzy Match)**
Used when strong identifiers are missing:
- Name similarity (“Sindhu T.” vs “Sindhu Thokala”)  
- Address similarity  
- IP/device fingerprint similarity  
- Behavioral correlation (patterns of usage)

Scoring model:
- Name score  
- Address score  
- Device score  
- Behavioral score  
→ Weighted to compute a single match confidence.

---

### **3. Cross-Device Resolution**
Linking:
- Web cookies  
- Mobile device IDs  
- App login tokens  
- Session identifiers  
To understand multi-device journeys.

---

## 🔗 Identity Graph Structure
Each customer is represented as a *node*, with edges describing relationships.

### Graph Components:
- **Entities**: customer nodes, device nodes, email nodes  
- **Edges**: relationships (logged in from, owns device, shares address)  
- **Stitching Events**: merge events stored as lineage  

---

## 🧬 Survivorship Logic
Rules that decide which field "wins" during merge:

- Most recently updated  
- Most trusted source system  
- Most complete value  
- Highest confidence score (probabilistic)  

Example:
- Email from CRM beats email from POS  
- Name from Loyalty beats name from anonymous web event  

---

## 🔎 ASCII Flow Diagram

[ Raw Layer Data ]
|
v
[ Identity Matching Engine ]
deterministic + probabilistic
|
v
[ Identity Graph ]
|
v
[ Unified Customer ID ]

---

## ✅ Outputs of Identity Resolution
- Stable **Customer360_ID**  
- One unified profile per person  
- Linkage of devices, behaviors, and interactions  
- Historical lineage preserved  
