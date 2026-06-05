 Marketing Channel Performance Dashboard

## Project Overview

This project was developed as part of  Marketing Analyst Technical Assignment. The objective was to integrate advertising data from Facebook Ads, Google Ads, and TikTok Ads into a unified marketing analytics dataset and build an interactive dashboard for cross-channel performance analysis.

This solution leverages Snowflake for data storage and transformation, SQL for data modeling, and Tableau Public for visualization.
## Business Problem

Marketing teams often manage advertising campaigns across multiple platforms. Since each platform provides data in different formats and structures, comparing campaign performance across channels becomes difficult.

This project addresses that challenge by creating a unified advertising dataset that enables centralized reporting and analysis across Facebook, Google Ads, and TikTok.

## Technology Stack

### Database

* Snowflake

### Query Language

* SQL

### Visualization

* Tableau Public

### Version Control

* GitHub

## Data Sources

The project uses the following datasets:

* 01_facebook_ads.csv
* 02_google_ads.csv
* 03_tiktok_ads.csv

Each dataset contains platform-specific campaign performance metrics.

## Snowflake Database Setup

### Database and Schema Creation

```sql


CREATE DATABASE ad_analytics;
CREATE SCHEMA ad_analytics.marketing;
USE DATABASE ad_analytics;
USE SCHEMA marketing;

### Source Tables

#### Facebook Ads Table

```sql
CREATE TABLE IF NOT EXISTS facebook_ads (
  date DATE,
  campaign_id VARCHAR,
  campaign_name VARCHAR,
  ad_set_id VARCHAR,
  ad_set_name VARCHAR,
  impressions INT,
  clicks INT,
  spend FLOAT,
  conversions INT,
  video_views INT,
  engagement_rate FLOAT,
  reach INT,
  frequency FLOAT
);
```

#### Google Ads Table

```sql
CREATE TABLE google_ads (
  date DATE,
  campaign_id VARCHAR,
  campaign_name VARCHAR,
  ad_group_id VARCHAR,
  ad_group_name VARCHAR,
  impressions INT,
  clicks INT,
  cost FLOAT,
  conversions INT,
  conversion_value FLOAT,
  ctr FLOAT,
  avg_cpc FLOAT,
  quality_score INT,
  search_impression_share FLOAT
);
```

#### TikTok Ads Table

```sql
CREATE TABLE tiktok_ads (
  date DATE,
  campaign_id VARCHAR,
  campaign_name VARCHAR,
  adgroup_id VARCHAR,
  adgroup_name VARCHAR,
  impressions INT,
  clicks INT,
  cost FLOAT,
  conversions INT,
  video_views INT,
  video_watch_25 INT,
  video_watch_50 INT,
  video_watch_75 INT,
  video_watch_100 INT,
  likes INT,
  shares INT,
  comments INT
);
```

---

## Data Ingestion Process

### Create Internal Stage

```sql
USE DATABASE AD_ANALYTICS;
USE SCHEMA MARKETING;

CREATE OR REPLACE STAGE ADS_STAGE;
```

### Validate Uploaded Files

```sql
LIST @ADS_STAGE;
```

### Load Facebook Data

```sql
COPY INTO facebook_ads
FROM @ADS_STAGE/01_facebook_ads.csv
FILE_FORMAT = (
TYPE = CSV
SKIP_HEADER = 1
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
);
```

### Load Google Ads Data

```sql
COPY INTO google_ads
FROM @ADS_STAGE/02_google_ads.csv
FILE_FORMAT = (
TYPE = CSV
SKIP_HEADER = 1
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
);
```

### Load TikTok Ads Data

```sql
COPY INTO tiktok_ads
FROM @ADS_STAGE/03_tiktok_ads.csv
FILE_FORMAT = (
TYPE = CSV
SKIP_HEADER = 1
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
);
```

### Data Validation

```sql
SELECT COUNT(*) FROM facebook_ads;
SELECT COUNT(*) FROM google_ads;
SELECT COUNT(*) FROM tiktok_ads;
```

---

## Unified Data Model

To support cross-platform reporting, the three advertising datasets were standardized into a single table called:

```sql
UNIFIED_ADS
```

The unified table combines:

* Facebook campaign data
* Google Ads campaign data
* TikTok campaign data

and standardizes metrics such as:

* Cost
* Impressions
* Clicks
* Conversions
* CTR
* CPC
* Cost Per Conversion
* ROAS
* Reach
* Video Views

The table was created using SQL transformations and UNION ALL operations.

---

## Validation Queries

```sql
SELECT COUNT(*) FROM unified_ads;

SELECT *
FROM unified_ads;
```

---

## Dashboard Overview

Dashboard Title:

### Channel Marketing Performance Dashboard

The Tableau dashboard includes:

### Campaign Performance Overview

Provides campaign-level visibility into:

* Campaign Name
* Ad Group Name
* CTR
* CPC
* Clicks
* Platform

### Top Performing Campaigns

Identifies campaigns generating the highest conversion volume.

### Daily Advertising Spend Trend by Platform

Tracks spending behavior over time across advertising channels.

### Conversions vs Advertising Spend by Platform

Compares marketing investment against conversion outcomes.

### Cost vs Conversion Analysis

Evaluates campaign efficiency and identifies optimization opportunities.

### Platform Engagement Rate

Measures audience engagement performance across channels.

### Platform Average CTR Analysis

Compares click-through performance between advertising platforms.

### Spend by Platform

Analyzes advertising budget allocation across Facebook, Google Ads, and TikTok.

---

## Key Metrics

The dashboard analyzes:

* Cost
* Impressions
* Clicks
* Conversions
* CTR
* CPC
* Cost Per Conversion
* Conversion Value
* ROAS
* Engagement Rate
* Reach
* Video Views

---

## Key Insights

* Unified reporting simplifies cross-channel marketing analysis.
* Cost versus Conversion analysis highlights campaign efficiency.
* Top-performing campaigns can be identified for scaling opportunities.
* Daily spend trends reveal budget pacing behavior.
* CTR and engagement metrics provide insight into audience interaction and campaign effectiveness.

## Project Outcome

This project demonstrates:

* Snowflake data ingestion and staging
* SQL-based data transformation
* Unified marketing data modeling
* Cross-channel campaign analytics
* Tableau dashboard development
* Marketing performance reporting

The final solution provides a centralized view of advertising performance across Facebook, Google Ads, and TikTok, enabling data-driven marketing decision making.
