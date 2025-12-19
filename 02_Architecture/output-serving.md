# 📤 Output Serving

This document explains how the final transformed data is made accessible to downstream consumers — analysts, applications, APIs, or external partners.

## 🧾 Output Types
List and describe each output format and destination.

| Output Name        | Type         | Consumer(s)         | Description                          |
|--------------------|--------------|---------------------|--------------------------------------|
| `analytics_dashboard` | BI Dashboard | Product, Marketing  | Key metrics for product usage        |
| `fct_sessions`     | Warehouse Table | Analysts, ML models | Session-level fact table             |
| `user_profile_api` | REST API     | External Partners    | Real-time user enrichment endpoint   |
| `daily_partner_feed.csv` | File Export | Partner A         | Daily export of campaign data        |


## 📍 Delivery Mechanisms
Describe how and where data is made available:

- **Data warehouse:** Final models published to `prod` schema
- **BI tools:** Connected to certified tables
- **APIs:** Expose key metrics via microservices or API gateways
- **File exports:** Scheduled delivery via S3, GCS, or SFTP


## 🔄 Update Frequency

| Output              | Refresh Schedule    | Trigger Type   |
|---------------------|---------------------|----------------|
| `fct_sessions`      | Hourly              | DAG schedule   |
| `analytics_dashboard` | Daily              | Downstream dependency |
| `daily_partner_feed.csv` | Daily at 3 AM    | Batch job      |


## 🛑 SLAs & Availability
Define expectations for output freshness and uptime.

| Output              | SLA (Latency)     | Criticality Level |
|---------------------|------------------|-------------------|
| `analytics_dashboard` | ≤ 1 hour         | High              |
| `fct_sessions`      | ≤ 2 hours         | Medium            |
| `partner_feed.csv`  | By 9 AM daily     | High              |


## 🔐 Access Controls
- Who can view or query each output?
- Is row-level or column-level security applied?
- Is PII exposed? If so, who has access?

> Example:  
> `dim_users.email` is masked for everyone except members of the Analytics Admin group.

## 📊 Monitoring
Describe how outputs are monitored for reliability:

- Freshness alerts for late data
- Volume checks (e.g., row count drift)
- BI tool dashboards showing update timestamps
- Logged delivery attempts for files/APIs


## 📦 Versioning & Deprecation
- How are changes to outputs communicated?  
- Are deprecated tables retained for a period?
- Versioning strategy (e.g., `v1`, `v2`, date-stamped tables)


> *Keep this doc current as new outputs are added or delivery methods change. It's the key reference for how data is served and trusted by end users.*
