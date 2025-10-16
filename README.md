# 📊 Customer NPS and Issue Resolution Analysis

This repository contains the data, analysis, and visualizations for a project focused on evaluating Customer NPS (Net Promoter Score), analyzing issue resolution efficiency, and assessing team performance. The goal is to provide actionable insights into customer satisfaction and operational performance, linking customer feedback (NPS rating) directly to service delivery metrics (resolution time, volume).

## 🎯 Agenda and Objective

The primary objective of this project is to create a dynamic, single-source-of-truth dashboard that helps stakeholders:

- Measure Customer Sentiment: Calculate and track the overall NPS score and its breakdown (Promoters, Passives, Detractors).
- Identify Pain Points: Determine which specific issue categories are driving the highest (or lowest) NPS scores.
- Evaluate Operational Efficiency: Monitor key service delivery metrics, including ticket volume trends, resolution times, and team performance.
- Connect Feedback to Action: Provide an analytical foundation to link low NPS scores with specific, quantifiable resolution inefficiencies, allowing management to prioritize improvements.

## 📦 Dataset Explanation

The analysis is built upon the Synthetic_NPS_data.csv file, which simulates customer service interaction and feedback data.

| Column Name | Description | Key Metric |
|--------------|--------------|-------------|
| Ticket_No | Unique identifier for each customer issue. | Volume Tracking |
| Created_Date / Time | Timestamp when the issue was logged. | Trend Analysis |
| Resolved_Date / Time | Timestamp when the issue was resolved. | Resolution Time Calculation |
| Assigned_To | The team or agent responsible for resolution. | Team Performance |
| Issue_2_NPS | The specific category or type of customer issue (e.g., "False Promise By Sales Team (NPS)"). | Segmentation |
| NPS_Rating | Customer's rating (0-10) of the resolution experience. | Core NPS Calculation |
| Status / Sub_Status | Current state of the ticket (e.g., Complete, Solved, Pending, Unresolved). | Efficiency Index |
| Duration | The number of days taken to resolve the issue. | Resolution Time Metrics |

## 💻 Power BI Dashboard Overview

The dashboard is structured into three distinct pages, each focusing on a specific area of analysis.

### 1. Net Promoter Score

This page is the central hub for measuring customer satisfaction using the NPS framework.

| Chart Title | Purpose | Key Insight Provided |
|--------------|----------|----------------------|
| Overall NPS Score | A KPI card displaying the final calculated NPS score. | The single metric for overall customer loyalty. |
| Promoters / Passives / Detractors Distribution | A donut or gauge chart showing the percentage breakdown of customer responses. | Visual representation of customer segments: Promoters (9-10), Passives (7-8), and Detractors (0-6). |
| Issue-wise NPS Score | A bar chart displaying the NPS score segmented by the Issue_2_NPS category. | Identifies which specific issues are most/least satisfying customers, enabling root cause analysis. |

### 2. Issues Vol. & Resolution Efficiency

This page focuses on the operational health of the issue resolution process.

| Chart Title | Purpose | Key Insight Provided |
|--------------|----------|----------------------|
| Weekly/Monthly Ticket Volume | A time-series chart showing the trend in incoming ticket volume. | Tracks demand and highlights seasonal or period-based load spikes. |
| Issue Volume | A chart showing the absolute count of tickets per Issue_2_NPS category. | Reveals the most common types of issues customers face, regardless of satisfaction. |
| Average Resolution Time (ART) + Min + Max | A detailed visual comparing the average, minimum, and maximum resolution time for each issue category. | Measures service speed and highlights issues with high variability (risk of long resolution times). |
| Unresolved Tickets % | A KPI showing the percentage of open/pending tickets. | Measures the backlog and the effectiveness of the team in closing tickets promptly. |

### 3. Team Performance

This page provides an internal view of how teams are performing against key efficiency metrics.

| Chart Title | Purpose | Key Insight Provided |
|--------------|----------|----------------------|
| Resolution Efficiency Index | A composite score or KPI card measuring a team's effectiveness (e.g., a ratio of tickets resolved on-time vs. total tickets). | A holistic score to rank and compare team efficacy. |
| Teams Ticket Vol. Trend | A trend chart showing ticket volume handled, segmented by the team. | Tracks workload distribution and capacity planning needs across teams. |
| Teams Performance | A visual comparing key metrics (like average resolution duration or resolution rate) across different teams. | Benchmarks performance between teams and identifies best practices or training needs. |

## 🛠️ Data Modeling and Calculation Logic

### Data Model Design

The model is designed around a single, denormalized Fact Table containing all transaction-level data from the Synthetic_NPS_data file.

The key elements of the model are:

- Fact Table (Sheet1 / NPS_Data): Contains all ticket and NPS transaction details.
- Calculated Columns/Measures: All metrics are derived from this single table, ensuring simplicity and performance. No complex relationship joins were required for this specific dataset.

### SQL Usage (NPS_Sql.pdf)

The SQL queries were used for initial data preparation and specific aggregations. Key functions included:

- NPS Calculation: Using COUNT and subqueries (or CTEs) to pre-calculate the total counts of Promoters, Passives, and Detractors based on the NPS_Rating groups (IN (9,10), IN (7,8), IN (0..6)).
- Issue-wise Aggregation: Grouping data by Issue_2_NPS to determine individual issue volumes and NPS components for subsequent analysis.

### DAX (Data Analysis Expressions) Usage (Nps_Dax.pdf)

DAX was essential for creating dynamic, filter-context-aware measures within the Power BI report.

| DAX Measure | Functionality | Calculation Logic |
|--------------|---------------|-------------------|
| NPS Score | Calculates the final NPS score for any selected context (overall, by issue, by team). | ( % Promoters - % Detractors ) |
| Per_Promoters | Calculates the percentage of Promoter responses. | DIVIDE(CALCULATE(COUNTROWS(), 'Sheet1'[NPS Rating] IN {9, 10}), COUNTROWS('Sheet1')) |
| Per_Detractors | Calculates the percentage of Detractor responses. | DIVIDE(CALCULATE(COUNTROWS(), 'Sheet1'[NPS Rating] IN {0, ..., 6}), COUNTROWS('Sheet1')) |
| Per_Passive | Calculates the percentage of Passive responses. | DIVIDE(CALCULATE(COUNTROWS(), 'Sheet1'[NPS Rating] IN {7, 8}), COUNTROWS('Sheet1')) |

## 📝 Summary

This project successfully integrates customer feedback with operational performance data. By calculating the NPS and segmenting it by specific issue type, the analysis identifies that the current overall NPS is 11.00, indicating a slight surplus of Promoters over Detractors, but with a significant opportunity to convert Passives into Promoters.

The analysis of resolution efficiency highlights where time and resource allocation can be optimized to directly impact customer satisfaction scores, providing a clear roadmap for service improvement initiatives based on both what customers are complaining about (Issue Volume) and how well the service teams are responding (Resolution Time, Team Performance).
