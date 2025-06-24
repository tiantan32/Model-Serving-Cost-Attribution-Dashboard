# Unlocking AI Observability: A Deep Dive into Databricks Model Serving Cost Attribution Dashboard

**Authors:**  
Tian Tan (Senior Solution Architect @ Databricks), Ashwin Srikant (Senior Solutions Architect @ Databricks), Jai Behl (Senior Solutions Architect @ Databricks)

> **Note:**  
> The dashboard linked here is Version 1 of what we hope to refine over time, to fully capture GenAI Usage and Cost Analysis. Please reach out to the authors if you would like to see a specific metric or dashboard that isn’t currently available.  
> **[Import the dashboard from this hyperlink!](https://github.com/tiantan32/Model-Serving-Cost-Attribution-Dashboard/blob/main/Model%20Serving%20Cost%20Attribution.lvdash.json)**

---

## Introduction

As organizations scale AI initiatives on Databricks, the need for comprehensive observability becomes paramount. Model serving costs can quickly spiral out of control without proper monitoring, and understanding usage patterns across Databricks’ 3 different serving modalities (pay-per-token, provisioned throughput, and batch inference) is crucial for optimization and chargeback.

The **Model Serving Cost Attribution Dashboard** represents a data-driven approach to AI observability. It leverages Databricks’ Unity Catalog system tables to provide visibility into model serving costs, usage patterns, and performance metrics. This technical deep dive explores how this dashboard transforms raw billing and usage data into actionable insights for FinOps, ML engineers, and data platform teams.

---

## Dashboard Overview

This dashboard is split into 4 sections:

1. **[Part I] Serving Summary**  - Monitor Model Serving Endpoint costs for all serving endpoints, analyze costs by users, size, and track token usage breakdown by models.

2. **[Part II] Pay-Per-Token**  - Monitor pay-per-token endpoint usage by analyzing token efficiency, comparing model cost-performance, and tracking usage trends.

3. **[Part III] Provisioned Throughput**  - Analyze historical usage patterns for capacity planning, provide right-sizing recommendations for endpoints that are over- or under-provisioned, and offer breakdowns of popular endpoints.

4. **[Part IV] Batch Inference** - Monitor batch inference usage over time across various compute types, analyze cost by users, and track resource utilization to optimize capacity planning. By leveraging these insights, teams can reduce unnecessary spending and ensure efficient use of infrastructure for large-scale AI workloads.

---

## Prerequisites

- A Databricks environment with access to system tables
- The required system tables are enabled (listed below)
- Import the repo
- Import the dashboard, edit your default parameters, and publish!

---

## The Power of Unity Catalog System Tables

These dashboards are powered by Databricks’ Unity Catalog system tables — a collection of automatically populated tables that capture comprehensive metadata about your Databricks operations. For model serving observability, nine key system tables provide the foundation:

### Billing & Cost Attribution

- `system.billing.usage`: The cornerstone table containing DBU consumption, cost data, and usage metadata
- `system.billing.list_prices`: Dynamic pricing information for accurate cost calculations

### Model Serving Analytics

- `system.serving.endpoint_usage`: Request-level data including token counts, latency, and response metrics
- `system.serving.served_entities`: Endpoint configuration, model metadata, and serving type information

### Compute & Workflow Integration

- `system.compute.warehouses`: SQL warehouse details for batch inference attribution
- `system.lakeflow.jobs`: Job execution data for batch workload tracking
- `system.lakeflow.pipelines`: DLT pipeline information for data engineering workloads
- `system.compute.clusters`: Non-serverless cluster details for batch inference attribution

### Governance & Access

- `system.access.workspaces_latest`: Workspace attribution for multi-tenant environments
- `system.query.history`: AI function usage patterns in SQL queries

---

## Dashboard Deep Dive — Serving Summary


In this dashboard, we can analyze costs and usage for all model serving endpoints in a single window, track costs, monitor, and optimize the organization’s AI infrastructure spend. Here is a high-level breakdown of each section:

### Serving Summary

![Databricks Model Serving Cost Attribution Dashboard](https://miro.medium.com/v2/resize:fit:720/format:webp/0*AjpC1PfKESKkO25v)
This tab monitors:

#### 1. Total Spend and Usage Metrics
- Displays total dollars spent, batch inference spend, provisioned throughput spend, pay-per-token spend, and total DBUs consumed, giving a high-level overview of resource utilization and costs.

#### 2. Endpoint and Model Analysis
- Use the **TopK filter** to identify the top endpoints by spend, allowing users to pinpoint which services or models are driving the most cost.
- Breaks down token usage by model type, helping teams understand which models are most heavily used and where optimization may be needed.
- **Note:** Usage Tracking needs to be enabled through AI gateway in the Model Serving Endpoint Configuration to track token usage.

#### 3. Serving Type and Cost Breakdown
- Visualizes the distribution of spending across different serving types (provisioned throughput, batch inference, Pay-Per-Token, CPU models, GPU models, etc.), supporting cost allocation and efficiency analysis.

  
![Databricks Model Serving Cost Attribution Dashboard](https://miro.medium.com/v2/resize:fit:720/format:webp/0*IWkp9_9H5PMyM6Zb)
#### 4. Weekly model serving spend across workspaces
- Display weekly spend on model serving, broken down by workspace, and allow users to see where the resources are being consumed and how spend changes over time

#### 5. Daily model serving spend by model serving types
- Display daily spend on model serving, broken down by model serving types, and identify potential optimization opportunities

  
![Databricks Model Serving Cost Attribution Dashboard](https://miro.medium.com/v2/resize:fit:720/format:webp/0*PwRSe52qRUEqmgqE)
#### 6. Top user list by token consumption
- Use TopK filter to identify the top users’ usage by token count, allowing users to pinpoint which services or models are being heavily used.
Note: Usage Tracking needs to be enabled through AI Gateway in the Model Serving Endpoint Configuration to track token usage

#### 7. Weekly number of requests by workspaces
- Displays weekly trends in the number of requests by workspace, making it easy to track usage patterns over time and identify periods of high or low activity.

![Databricks Model Serving Cost Attribution Dashboard](https://miro.medium.com/v2/resize:fit:720/format:webp/0*WI35_rB1bEE-3-YS)

This table provides a detailed breakdown of the dollar costs associated with various model endpoints used across different workspaces. It lists the top endpoints by total cost, showing how much each endpoint has incurred in terms of provisioning, batch inference, and other model serving types, culminating in a “Grand Total” for each endpoint. This allows teams to see exactly where their model serving budget is being spent, broken down by endpoint and workspace, which helps identify high-cost services and enables future resource optimization.

![Databricks Model Serving Cost Attribution Dashboard](https://miro.medium.com/v2/resize:fit:720/format:webp/0*70KoXiCwYTdI4J10)
AI Functions enable users to seamlessly integrate large language models into their SQL workflows, allowing them to perform tasks such as sentiment analysis, text summarization, and translation directly within their data queries. This chart addresses observability needs by displaying the number of queries made to each AI function, highlighting which functions are most frequently used.

## Conclusion
The Databricks Model Serving Cost Attribution Dashboard represents a paradigm shift in AI observability, demonstrating how Unity Catalog system tables can be transformed into powerful business intelligence. By providing comprehensive visibility into costs, usage patterns, and performance metrics across all serving types, this solution empowers organizations to optimize their AI investments while maintaining operational excellence.

For organizations implementing similar solutions, the patterns and techniques demonstrated here provide a robust foundation for building comprehensive AI observability capabilities. The key lies in leveraging the rich metadata available in system tables while designing flexible, extensible architectures that can evolve with changing business needs.

## What’s Next: Serving Type Breakdown
Looking ahead, our next focus will be on developing Part II: a detailed breakdown of serving types, including pay-per-token, provisioned throughput, and batch inference. This phase will involve building comprehensive views for each serving type, allowing us to analyze cost drivers, usage patterns, and performance metrics specific to each model serving method. By providing granular insights into how resources are consumed across different serving types, we aim to further enhance transparency and empower teams to make informed decisions about optimization and resource allocation.

---
_This dashboard solution is available as an open-source reference implementation. It enables organizations to jumpstart their AI observability initiatives while customizing for specific requirements. For changes, bugs, or feature requests, comment on this article or message us directly._
