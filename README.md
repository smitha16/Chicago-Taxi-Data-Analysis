Chicago TNC Scalability Analysis with PySpark
A distributed data engineering project analyzing Chicago rideshare (TNC) trip data at scale using PySpark, benchmarking query performance across 100K, 500K, and 1M rows to demonstrate sub-linear scaling behavior.

Overview
This project pulls live trip data from the City of Chicago Open Data Portal and runs a suite of analytical queries to uncover urban mobility patterns — while systematically measuring how query execution time scales with data volume.
The core finding: Spark achieves sub-linear scaling across all queries, meaning when data grows 10×, query time grows by significantly less than 10×. This validates the efficiency of Spark's Catalyst optimizer and lazy evaluation model for medium-to-large scale analytics.

Dataset

Source: City of Chicago — Transportation Network Providers (TNC) Trip Data
Scales analyzed: 100K rows, 500K rows, 1M rows
Key fields: trip start/end timestamps, pickup/dropoff community areas, trip miles, fares, shared trip flag


Queries
Baseline Queries (Q1–Q5)
QueryDescriptionQ1Peak Hour Analysis — hourly trip volume distributionQ2Top Pickup Areas — top 10 community areas by trip count, fare, distanceQ3Fare vs Distance — average fare bucketed by distance rangeQ4Day of Week Patterns — normalized demand across weekdaysQ5Shared vs Solo Economics — fare, distance, duration comparison
Advanced Queries (AQ1–AQ3)
QueryDescriptionAQ1Origin–Destination Flow — trip volume between every community area pairAQ2Driver Supply Approximation — hourly imbalance score using distinct pickup area counts as supply proxyAQ3Community-Area Entropy — Shannon entropy of destination distributions per pickup area

Key Findings

Scaling efficiency: Data grew 10×; average query time grew only ~2–3×, demonstrating strong sub-linear scaling
Peak demand: Consistent morning (8–9 AM) and evening (5–6 PM) spikes across all dataset sizes
Top demand hubs: Near North Side, Loop, and O'Hare dominate across all scales
Mobility entropy: High-entropy areas (Loop, Near North Side) show dispersed destination patterns; residential neighborhoods show concentrated flows — a stable geographic mobility signature
UDF bottleneck: Community area name mapping via Python UDF was the worst-scaling operation — a broadcast join would be the production fix


Visualizations

Normalized peak hour curves (area fill + line)
Grouped horizontal bar charts for top pickup areas
Fare vs distance grouped bar charts
Day-of-week violin + scatter plots
Circular OD flow wheels per dataset scale
Diverging bar chart for driver supply imbalance
Entropy distribution violin + strip plots


Tech Stack

PySpark — distributed query execution
Python / Pandas — data ingestion, chunked downloads
Matplotlib / Seaborn — visualizations
Google Colab — execution environment (local[*] Spark master, 4GB driver memory)

Setup & Usage
bash# Install dependencies
pip install pyspark psutil pandas matplotlib seaborn

# The notebook handles all data download and Spark setup automatically
# Just run all cells in order
