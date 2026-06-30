# Project 25: Weather-Based Activity Recommendation Engine

## Project Goal
The primary goal of this project is to build an autonomous expert system that recommends outdoor and indoor activities based on current and historical weather conditions. The system utilizes an automated data processing pipeline to collect data from a shared Weather REST API, separates raw and curated data in cloud storage, and leverages a distributed Machine Learning classifier (Random Forest) to perform multi-criteria scoring and rank the best activities per station and time.

## Architecture Design
The solution is deployed on an AWS EMR cluster using PySpark, following a clear, layer-based data processing workflow:

1. **REST API Collector:** Python scripts fetch batch measurements securely using token authentication from 4 stations in the Tricity area (GDN_01, GDN_02, SOP_01, GBA_01).
2. **Raw Storage (Ingestion):** Unaltered JSON responses are persisted in the `s3://.../raw_data/` layer.
3. **Processing & Validation:** PySpark DataFrames are used to cast types, handle missing values, and generate synthetic labels for 10 diverse activities based on deterministic weather rules.
4. **Curated Dataset:** Cleaned and labeled data is saved in an optimized Parquet format to `s3://.../curated_data/`.
5. **Analytics & ML:** A Random Forest Classifier is trained on the curated dataset using PySpark MLlib. 
6. **Inference & Analytical Output:** The system exposes interactive functions for scenario testing and station-specific querying. Final recommendations with natural language explanations are appended to `s3://.../analytical_output/`.

## Execution Steps

To reproduce the pipeline and run the recommendation engine:

1. **Environment Setup:** Launch an AWS EMR cluster (or local PySpark environment) and ensure the `weather_based_activity.ipynb` notebook is attached to an active Spark session.
2. **Step 1 (Ingestion):** Run the first cell to extract data from the Weather REST API and load the raw JSON files into the S3 bucket.
3. **Step 2 (Data Processing):** Execute the second cell to clean the raw data, apply activity rules (synthetic labeling), and save the curated Parquet files.
4. **Step 3 (Model Training):** Run the third cell to split the data, build the ML Pipeline (`StringIndexer`, `VectorAssembler`, `RandomForestClassifier`), and train the model.
5. **Step 4 (Application & Scenario Tests):** Run the interactive functions to test specific weather scenarios or retrieve the top 3 ranked recommendations (with explanations) for a chosen station.
6. **Step 5 (Metrics & Dashboard):** Execute the final cell to generate the analytical report, including the Feature Importance chart, class distribution table, and model accuracy metrics.
