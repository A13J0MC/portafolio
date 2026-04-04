---
title: "Modern Data Warehouse with dbt & Snowflake"
description: "A fully modeled analytics data warehouse built with dbt on Snowflake, with automated testing, documentation, and CI/CD deployment via GitHub Actions."
icon: "🏗️"
category: "Data Warehouse"
status: completed
featured: true
tech_stack:
  - dbt
  - Snowflake
  - Python
  - GitHub Actions
  - SQL
  - Fivetran
github: "https://github.com/AlejandroMendoza/dbt-warehouse"
---

## Overview

A production analytics warehouse serving 20+ analysts across the company. Built with dbt on Snowflake using a layered architecture (staging → intermediate → marts), with automated data testing, model documentation, and CI/CD deployment pipelines.

## dbt Project Structure

```
models/
├── staging/          # 1:1 source mapping, light cleaning
│   ├── salesforce/
│   ├── stripe/
│   └── postgresql/
├── intermediate/     # Business logic transformations
│   └── finance/
└── marts/            # Final consumption-ready models
    ├── finance/
    │   ├── fct_revenue.sql
    │   └── dim_customer.sql
    └── marketing/
        ├── fct_campaigns.sql
        └── dim_channels.sql
```

## Key Features

- **400+ dbt models** covering Sales, Finance, Marketing, and Product domains.
- **Automated testing** — 1,200+ data tests (uniqueness, not-null, referential integrity, custom SQL assertions) run on every PR.
- **Auto-generated docs** — `dbt docs generate` publishes a searchable data catalog on merge to main.
- **CI/CD with GitHub Actions** — PRs run `dbt test` on slim CI (only changed models + descendants). Merges trigger full production runs.
- **Row-level security** — Snowflake row access policies applied to sensitive columns via dbt post-hooks.

## Sample dbt Model

```sql
-- models/marts/finance/fct_revenue.sql
{{ config(
    materialized='incremental',
    unique_key='payment_id',
    cluster_by=['payment_date']
) }}

with payments as (
    select * from {{ ref('stg_stripe__payments') }}
    {% if is_incremental() %}
    where payment_date > (select max(payment_date) from {{ this }})
    {% endif %}
),

customers as (
    select * from {{ ref('dim_customer') }}
),

final as (
    select
        p.payment_id,
        p.payment_date,
        p.amount_usd,
        c.customer_segment,
        c.acquisition_channel
    from payments p
    left join customers c using (customer_id)
)

select * from final
```

## Results

- Reduced analyst time spent on data prep by **40%** through standardized, trusted marts.
- Data freshness SLA: all marts updated within **1 hour** of source ingestion.
- **Zero production incidents** caused by untested model changes (enforced via CI blocking checks).
