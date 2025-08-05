
# Crime Analysis & Prediction

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)  [![Build Status](https://img.shields.io/badge/Notebook-Ready-yellow)]()

---

## Overview

**Crime Analysis & Prediction** is a comprehensive data-science project that explores Chicago crime data (2018–2019), uncovers temporal and spatial patterns, and builds predictive models for arrest likelihood. It covers:

1. Data ingestion & cleaning  
2. Feature engineering  
3. Exploratory data analysis  
4. Geospatial visualization  
5. Supervised modeling & evaluation  
6. (Optional) Unsupervised clustering  

This README describes the dataset, preprocessing steps, feature transformations, EDA findings, modeling workflow, results, and how to run the project.

---

## Dataset

- **Source file:** `Crimes_-_2001_to_Present_20250805.csv`  
- **Rows:** ~530,000 records (before cleaning)  
- **Columns:**  
  - **Identifiers & metadata:** `id`, `case_number`, `date`, `updated_on`, `fbi_code`, `iucr`  
  - **Location data:** `block`, `location_description`, `latitude`, `longitude`, `x_coordinate`, `y_coordinate`, `district`, `ward`, `community_area`, `beat`  
  - **Crime details:** `primary_type`, `description`, `arrest` (bool), `domestic` (bool)  

> **Note:** The raw file must be placed in the project root (same folder as the notebook).

---

## Environment & Installation

1. **Clone the repo**  
   ```bash
   git clone https://github.com/Akshit-Soni/Crime_Analysis_and_Prediction.git
   cd Crime_Analysis_and_Prediction
   ```

2. **Create & activate a virtual environment**  
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Launch the Jupyter notebook**  
   ```bash
   jupyter notebook analysis_of_crimes.ipynb
   ```

---

## Data Cleaning & Preprocessing

1. **Column standardization**  
   - Strip whitespace, remove commas/spaces, convert to lowercase, replace spaces with underscores.  
2. **Drop duplicates & irrelevant columns**  
   - Removed `id`, `case_number`, combined `location`.  
3. **Missing-value handling**  
   - Visualize with `missingno` heatmap & dendrogram.  
   - Drop rows where `latitude` (and co-missing fields) is null.  
   - Drop remaining rows with any null in `ward` or `community_area`.  
   - Final size: ~520,000 rows.  

---

##  Feature Engineering

- **Datetime features**  
  - Convert `date` → `datetime`; extract `day_of_week`, `month`, `hour` (`time`).  
- **Season mapping**  
  - Map months → Spring/Summer/Fall/Winter.  
- **Crime type grouping**  
  - Aggregate granular `primary_type` into 8 broad categories (THEFT, ASSAULT, etc.).  
- **Location grouping**  
  - Collapse `location_description` into 8 buckets (RESIDENCE, BUSINESS, PUBLIC_AREA, etc.).  
- **Zone extraction**  
  - Derive `zone` (North/South/East/West) from block prefixes.  
- **Boolean flags**  
  - Convert `arrest` & `domestic` → 0/1 for numeric analysis.  
- **Categorical conversion**  
  - Convert ID-like fields (`year`, `time`, `ward`, etc.) to `pd.Categorical`.  

---

## Exploratory Data Analysis (EDA)

1. **Temporal trends**  
   - **Weekday:** Uniform distribution; slight decrease in 2019.  
   - **Month/Season:** Peak in May–August; winter months see fewer crimes.  
   - **Hourly:** Spike around midnight and afternoon lull.  

2. **Spatial patterns**  
   - **Zones:** South side highest crime density; East side lowest.  
   - **Residences & public areas** account for ~70% of incidents.  
   - **District maps:** Scatter plots of (X, Y) coordinates colored by district & crime type reveal hot spots.

3. **Crime composition**  
   - **Pie charts:** THEFT & NON-CRIMINAL_ASSAULT ~60% of total.  
   - **Bar plots:** Top 20 crime types confirm dominance of THEFT and BATTERY.

4. **Arrest analysis**  
   - Only ~25% of crimes lead to arrest; steady across both years.  
   - Arrest rates vary by crime type (higher for certain violent categories).

---

## Modeling & Results

### 1. Data Preparation for Modeling

- **Drop** text & geospatial fields: `date`, `block`, `fbi_code`, `x_coordinate`, `y_coordinate`, etc.  
- **One-hot encode** all categorical features (`get_dummies(drop_first=True)`).  
- **Train/test split:** 70% train, 30% test (`random_state=42`).  
- **Scaling:** StandardScaler on all features.

### 2. Classifiers Evaluated

| Model                   | Key Strengths                          | Expected Performance  |
|-------------------------|----------------------------------------|-----------------------|
| Gaussian Naïve Bayes    | Fast; handles binary features well     | Moderate accuracy     |
| Logistic Regression     | Interpretable; baseline linear model   | Good baseline         |
| Decision Tree           | Captures non-linearities; interpretable| Overfit risk          |
| Random Forest           | Ensemble; robust generalization        | Best overall          |

### 3. Evaluation Metrics

- **Accuracy**, **Precision**, **Recall**, **F1-score** per class.  
- **Confusion Matrix** and **ROC curves** for binary arrest prediction.

> **Example result:**  
> Random Forest achieved ~86% accuracy, F1-score of 0.75 on the positive (arrest) class.

---

## Unsupervised Clustering (Optional)

- **Hierarchical clustering** (dendrogram) to identify correlated missing/value patterns.  
- **K-Means** & **DBSCAN** on selected features (time, location, zone) to discover crime clusters without labels.  
- Visualize cluster assignments on scatter maps to reveal natural “hot spots.”

---

## Usage

1. **Run the notebook**: Open `analysis_of_crimes.ipynb` and execute cells sequentially.  
2. **Modify parameters**:  
   - Adjust clustering `n_clusters` or DBSCAN `eps`/`min_samples`.  
   - Experiment with other classifiers or hyperparameters.  
3. **Extend analysis**:  
   - Incorporate socio-economic or weather data.  
   - Build a real-time dashboard with Streamlit or Dash.

---

## Contributing

Contributions are welcome! Please:

1. Fork this repository.  
2. Create a feature branch (e.g., `feature-enhancement`).  
3. Commit your changes & open a Pull Request.  

Be sure to follow existing code style and add descriptive markdown for any new analysis.

---

*Happy analyzing!* 
