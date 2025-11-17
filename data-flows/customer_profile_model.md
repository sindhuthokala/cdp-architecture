# 👤 Customer 360 Profile Data Model

This document defines the **unified customer profile schema** used in the Customer 360 Platform.  
Each row in this model represents a single resolved customer, identified by a persistent `customer360_id`.

The profile aggregates data from:
- CRM and loyalty systems  
- Web and mobile behavioral events  
- Ecommerce and POS transactions  
- Marketing platforms  
- Customer support and service systems  
- Enrichment and 3rd-party sources  

---

## 🎯 Goals of the Profile Model

- Provide a **single source of truth** for customer attributes  
- Support analytics, segmentation, personalization, and ML  
- Be stable, extensible, and vendor/platform neutral  
- Enable clear separation between **raw**, **modeled**, and **derived** attributes  

---

## 🧱 High-Level Structure

At a high level, each profile includes:

- Identity & keys  
- Core attributes (name, email, phone, etc.)  
- Demographics & preferences  
- Lifecycle & engagement metrics  
- Commerce & value metrics  
- AI / ML scores and segments  

---

## 📌 Core Fields

| Field Name         | Type        | Description |
|--------------------|------------|-------------|
| `customer360_id`   | STRING     | Persistent unified customer identifier used across all C360 tables. |
| `primary_source`   | STRING     | Most trusted system of record (e.g., `crm`, `loyalty`, `ecom`). |
| `source_ids`       | ARRAY/JSON | List of IDs from source systems (CRM ID, loyalty ID, web user ID, etc.). |
| `created_at`       | TIMESTAMP  | Timestamp when this profile was first created in C360. |
| `updated_at`       | TIMESTAMP  | Timestamp of last update to this profile. |

---

## 👤 Identity & Contact

| Field Name       | Type       | Description |
|------------------|-----------|-------------|
| `first_name`     | STRING    | Unified first name based on survivorship rules. |
| `last_name`      | STRING    | Unified last name based on survivorship rules. |
| `full_name`      | STRING    | Convenience concatenation of first and last name. |
| `primary_email`  | STRING    | Canonical email used for communication. |
| `emails`         | ARRAY     | List of known email addresses. |
| `primary_phone`  | STRING    | Canonical phone number (normalized). |
| `phones`         | ARRAY     | List of known phone numbers. |
| `language`       | STRING    | Preferred language (if available). |
| `country`        | STRING    | Country (ISO code preferred). |
| `region`         | STRING    | Region/State/Province. |
| `city`           | STRING    | City. |
| `postal_code`    | STRING    | Postal or ZIP code. |

---

## 🧬 Demographics & Preferences

| Field Name            | Type       | Description |
|-----------------------|-----------|-------------|
| `date_of_birth`       | DATE      | Date of birth (if available, may be partial). |
| `age`                 | INTEGER   | Derived age based on date_of_birth. |
| `gender`              | STRING    | Self-reported or modeled gender. |
| `preferred_channel`   | STRING    | Primary contact channel (email, sms, app, etc.). |
| `opt_in_status`       | STRING    | Marketing consent status (opted_in, opted_out, unknown). |
| `consent_scopes`      | ARRAY     | List of consent scopes (e.g., email_marketing, sms_offers). |
| `interests`           | ARRAY     | High-level interest categories (e.g., “outdoors”, “tech”). |

---

## 🔄 Lifecycle & Engagement

| Field Name              | Type       | Description |
|-------------------------|-----------|-------------|
| `first_seen_at`         | TIMESTAMP | First known interaction timestamp. |
| `last_seen_at`          | TIMESTAMP | Most recent interaction timestamp. |
| `last_channel_seen`     | STRING    | Last channel used (web, app, store, support). |
| `total_sessions`        | INTEGER   | Count of web/app sessions. |
| `total_interactions`    | INTEGER   | Count of events (views, clicks, opens, etc.). |
| `email_opens`           | INTEGER   | Number of email opens. |
| `email_clicks`          | INTEGER   | Number of email clicks. |
| `push_notifications`    | INTEGER   | Number of push messages sent. |
| `support_tickets`       | INTEGER   | Number of support tickets created. |
| `last_support_interaction_at` | TIMESTAMP | Last support touchpoint. |

---

## 💰 Commerce & Value Metrics

| Field Name                  | Type       | Description |
|-----------------------------|-----------|-------------|
| `first_purchase_at`         | TIMESTAMP | Timestamp of first known purchase. |
| `last_purchase_at`          | TIMESTAMP | Timestamp of most recent known purchase. |
| `orders_count`              | INTEGER   | Total number of completed orders. |
| `total_revenue`             | DECIMAL   | Sum of all order values. |
| `avg_order_value`           | DECIMAL   | Average order value (AOV). |
| `refunds_count`             | INTEGER   | Number of refunded orders. |
| `refunds_amount`            | DECIMAL   | Total refunded amount. |
| `lifetime_value`            | DECIMAL   | Modeled or calculated customer LTV. |
| `loyalty_tier`              | STRING    | Loyalty or membership tier (if applicable). |
| `loyalty_points_balance`    | DECIMAL   | Current loyalty or points balance. |

---

## 🤖 AI / ML & Segmentation Fields

| Field Name               | Type       | Description |
|--------------------------|-----------|-------------|
| `churn_risk_score`       | DECIMAL   | 0–1 or 0–100 score indicating likelihood to churn. |
| `engagement_score`       | DECIMAL   | Composite score based on recency, frequency, activity. |
| `propensity_to_buy_score`| DECIMAL   | Likelihood to purchase in a given time period. |
| `recommendation_vector`  | ARRAY/JSON| Encoded representation of product affinities. |
| `primary_segment`        | STRING    | Main assigned segment (e.g. "High value – at risk"). |
| `segments`               | ARRAY     | List of all segment memberships. |

---

## 🧪 Example Record (Simplified JSON)

```json
{
  "customer360_id": "c360_12345",
  "primary_source": "crm",
  "source_ids": {
    "crm_id": "C123",
    "loyalty_id": "L987",
    "web_user_id": "U456"
  },
  "first_name": "Sindhu",
  "last_name": "Thokala",
  "primary_email": "sindhu@example.com",
  "country": "US",
  "preferred_channel": "email",
  "opt_in_status": "opted_in",
  "first_seen_at": "2021-05-10T13:45:00Z",
  "last_seen_at": "2024-07-01T09:20:00Z",
  "orders_count": 8,
  "total_revenue": 1250.75,
  "lifetime_value": 1450.00,
  "churn_risk_score": 0.27,
  "primary_segment": "High value – engaged"
}
