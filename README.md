VoltRelay Energy — Battery Swapping Network Analytics

Overview

This project analyzes a synthetic battery-swapping network operated by VoltRelay Energy, covering electric 2W and 3W users across six Indian cities: Bengaluru, Delhi NCR, Hyderabad, Pune, Mumbai, and Jaipur.

The dataset represents network activity from January 2024 to June 2025 with approximately 3.9 million swap attempts.

The objective is to understand network performance, service failures, station and battery performance, pricing and partner economics, customer support, and rider retention.

Business Questions

The analysis focuses on six core areas:

Network Performance

Completed swaps and revenue over time

Failure and completion rates

Revenue/contribution per completed swap

Service Failures & Customer Experience

Failed and abandoned swaps

Queue waiting time by station, hour, season, and vehicle class

Operational patterns associated with service failures

Station & Geographic Performance

Performance across cities and location types

Charger generation and station characteristics

Station age, competition, commissioning, and operating conditions

Battery & Equipment Performance

Battery State of Health (SOH)

Supplier and manufacturing-lot comparisons

Battery age, firmware, range, and swap behavior

Pricing & Partner Economics

Standard, partner, peak, and off-peak tariffs

Fleet partner contracts and discounts

Revenue and margin-related patterns

Rider Retention

Factors associated with riders returning or not returning

Primary versus contributing factors

Association is distinguished from causation

Datasets

Dataset

Rows

Purpose

swap_events.csv.gz

3,877,013

Swap attempts, outcomes, pricing, queue time, battery and station information

station_hourly_status.csv.gz

1,487,712

Hourly station inventory, charging, outages and telemetry

riders.csv

20,000

Rider profiles, plans, vehicles and acquisition information

batteries.csv

6,500

Battery specifications, suppliers, lots, SOH and lifecycle

support_tickets.csv

44,000

Customer complaints, support channels, resolution and CSAT

stations.csv

152

Station location, capacity, costs, charger generation and commissioning

city_daily_context.csv

3,282

Daily city-level context

fleet_partners.csv

12

Fleet partner contracts, discounts, vehicle class and active cities

Dataset Relationships

fleet_partners
      │
      ▼
    riders
      │
      ▼
 swap_events ─────────► stations
      │
      ▼
  batteries

riders ─────► support_tickets ─────► stations / batteries

stations + date ─────► city_daily_context

Notebook Structure

Nitin's datasets

fleet_partners

riders

stations

swap_events

Abhishek's datasets

batteries

city_daily_context

station_hourly_status

support_tickets

The merged notebook uses these standardized dataframe names:

fleet_partners
riders
stations
swap_events
batteries
city_daily_context
station_hourly_status
support_tickets

Analysis Workflow

Raw Data
   ↓
Data Understanding
   ↓
Data Quality Checks
   ↓
Cleaning & Validation
   ↓
Feature Engineering
   ↓
Standalone EDA
   ↓
Integrated Business Analysis
   ↓
Business Findings
   ↓
Final Report

Important Data Quality Handling

The dataset contains intentionally introduced data-quality issues.

Swap Event Timestamp Bug

For firmware v3.2.0, between March 10, 2025 and April 14, 2025, event timestamps are approximately 5 hours 30 minutes early.

The notebook preserves the original timestamp and creates a corrected analytical timestamp.

event_ts_original
        │
        └── preserved for auditability

event_ts_corrected
        │
        └── used for temporal analysis

Near-Duplicate / Retry Events

Some events occur very close together, particularly at poor-connectivity stations and in offline_batch mode.

These are not blindly removed because close-together events may represent legitimate retries or subsequent swap attempts.

Distance Outliers

km_since_last_swap contains negative and unusually large values, partly due to odometer resets.

These observations are retained and flagged rather than blindly deleted.

SOC / SOH Values Above 100

A small number of telemetry values exceed 100 due to sensor calibration drift. These are flagged rather than automatically discarded.

Missing Station Telemetry

Missing telemetry values in station_hourly_status are not automatically interpreted as zero. The telemetry_status field provides additional context.

Support Ticket CSAT

CSAT is missing for a large proportion of tickets and the missingness is not necessarily random. Therefore, a simple overall CSAT average can be biased.

Test Stations

STN-TST stations contain unusual zero-value transactions and should be deliberately included or excluded depending on the analysis.

Analytical Principle

The project distinguishes association from causation.

For example, if stations with longer queue times also show more abandoned swaps, the analysis can identify an association. It should not automatically claim that queue time caused abandonment without supporting evidence.

The same principle is applied to rider retention, station performance, battery health, and other business questions.

Technologies

Python

Pandas

NumPy

Matplotlib

Seaborn

Google Colab / Jupyter Notebook

The project uses Python-based analysis and does not require SQL or a dashboard.

How to Run

Google Colab

Open the merged notebook in Google Colab.

Upload or mount the required dataset files.

Update file paths if required.

Run the notebook sequentially from top to bottom.

Local Jupyter

Install the main dependencies:

pip install pandas numpy matplotlib seaborn jupyter

Then:

jupyter notebook

Open the merged notebook and execute the cells sequentially.

Expected Outputs

The analysis can produce:

Network performance trends

Swap completion and failure metrics

Queue-wait analysis

Station and city comparisons

Battery health and degradation analysis

Supplier and manufacturing-lot comparisons

Pricing and tariff analysis

Fleet partner economics

Customer support analysis

Rider retention analysis

Operational anomaly analysis

Business findings and recommendations

Project Deliverables

Google Colab/Jupyter Notebook — complete analysis

Analysis Report (PDF) — business-focused findings

Short Presentation/Video — concise explanation of the analysis and conclusions

Team

Nitin

Analysis of:

Fleet partners

Riders

Stations

Swap events

Abhishek

Analysis of:

Batteries

City daily context

Station hourly status

Support tickets

The individual notebooks were merged into a single workflow for integrated business analysis.

Disclaimer

This is a synthetic dataset created for an analytics challenge. Findings are specific to the simulated VoltRelay Energy network and should not be interpreted as real-world measurements.
