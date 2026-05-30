# Heart Disease Clustering — Cardiovascular Risk Segmentation

End-to-end unsupervised clustering pipeline segmenting 29,944 patients into actionable cardiovascular risk profiles using SAS Enterprise Miner

SAS Enterprise Miner | 29,944 patients | Ward's / Centroid / Avg Linkage | Status: Complete

---

## Project Overview

Cardiovascular disease is the leading cause of death globally. This project builds an unsupervised machine learning pipeline to discover natural patient groupings based purely on clinical and behavioral metrics — with no predefined labels.

The pipeline ingests raw clinical records, filters outliers, applies three hierarchical clustering algorithms in parallel, and surfaces three distinct patient risk segments.

Key outcomes:
- 3 statistically validated risk segments from 29,944 patient records
- Compared Ward's, Centroid, and Average Linkage clustering
- Used Cubic Clustering Criterion (CCC) for automatic segment count selection
- Identified age, BMI, diabetes, and walking difficulty as dominant risk factors

---

## Dataset

- Source: Clinical cardiovascular health survey
- Total Records: 29,944 patient observations
- Features: 18 clinical and behavioral attributes
- Format: Heart Disease.sas7bdat

### Features

| Feature | Type | Description |
|---------|------|-------------|
| HeartDisease | Binary | Has cardiovascular disease |
| BMI | Continuous | Body Mass Index |
| Smoking | Binary | Smoked 100+ cigarettes lifetime |
| AlcoholDrinking | Binary | Heavy alcohol consumption |
| Stroke | Binary | Prior stroke history |
| PhysicalHealth | Continuous | Days poor physical health (30 days) |
| MentalHealth | Continuous | Days poor mental health (30 days) |
| DiffWalking | Binary | Difficulty walking |
| Sex | Categorical | Biological sex |
| AgeCategory | Categorical | Age bracket (18-24 through 80+) |
| Race | Categorical | Race/ethnicity |
| Diabetic | Categorical | Diabetes status |
| PhysicalActivity | Binary | Active in past 30 days |
| GenHealth | Ordinal | Self-reported general health |
| SleepTime | Continuous | Average hours sleep per night |
| Asthma | Binary | Asthma diagnosis |
| KidneyDisease | Binary | Kidney disease diagnosis |
| SkinCancer | Binary | Skin cancer diagnosis |

---

## Pipeline Architecture

Raw Data (29,944 rows x 18 features)
-> Input Data Source Node (map roles, set measurement levels)
-> Filter Node (remove statistical outliers, STD-based)
-> Cluster Node 1: Ward's Minimum Variance (primary)
-> Cluster Node 2: Centroid Method (comparison)
-> Cluster Node 3: Average Linkage (comparison)
-> Reporter Node (segment profiles, CCC plots, statistics)
-> 3 Patient Risk Segments

---

## Step-By-Step Implementation

### Step 1 — Data Ingestion

Load Heart Disease.sas7bdat via an Input Data Source node.

- Target: HeartDisease (binary — post-hoc validation only, not used for clustering)
- Input: All 17 remaining features
- Measurement levels: Binary flags set to BINARY; BMI/PhysicalHealth/SleepTime set to INTERVAL

### Step 2 — Outlier Filtering

Add a Filter Node to remove extreme clinical values.

- Method: Standard deviation-based (STD)
- Threshold: Exclude records more than 3 standard deviations from the mean
- Why: A BMI of 95+ or 30 days of poor health every month would distort cluster centroids

### Step 3 — Parallel Cluster Analysis

Three Cluster Nodes run in parallel with different algorithms.

Normalization: STD method — standardizes all inputs before clustering so BMI (range 10-94) does not dominate binary (0/1) features.

| Method | How It Works | Strength |
|--------|-------------|----------|
| Ward's Minimum Variance | Minimizes within-cluster sum of squares | Compact, well-separated clusters |
| Centroid Method | Merges based on centroid distance | Fast on large datasets |
| Average Linkage | Average distance between all observation pairs | Robust to outliers |

Segment selection: AUTOMATIC via Cubic Clustering Criterion (CCC).
CCC > 3 = strong structure. CCC 2-3 = moderate. CCC < 2 = weak.

### Step 4 — Reporting

Reporter Node produces:
- Segment size statistics (count and percentage per cluster)
- Variable distribution plots per segment
- Mean / frequency tables
- CCC plot showing optimal cluster count

---

## Results — Three Patient Risk Segments

### Segment 1 — Low Risk Control Group

Physical Activity: High | BMI: Normal (18.5-24.9) | Self-Reported Health: Excellent to Good
Diabetes: Rare | Difficulty Walking: Very low | Smoking: Predominantly non-smokers

Clinical interpretation: Healthy baseline population. Minimal cardiovascular intervention needed.

### Segment 2 — High Age-Related Risk Group

Age: Heavily concentrated 80+ | Difficulty Walking: High prevalence
Physical Health: Poor | Physical Activity: Low | BMI: Moderate to high

Clinical interpretation: Elderly patients with functional decline driven by age rather than lifestyle. Focus on fall prevention, mobility support, comorbidity monitoring.

### Segment 3 — Metabolic and Lifestyle Risk Group

BMI: Elevated (26.5-31.0 avg) | Diabetic: High prevalence (borderline or confirmed)
Age: Middle-aged (45-64) | Smoking: Higher than Segment 1 | Physical Activity: Moderate to low

Clinical interpretation: Classic metabolic syndrome profile. Highest potential for lifestyle intervention, diabetes management, and preventive cardiovascular screening.

---

## Algorithm Comparison

| Metric | Ward's | Centroid | Average Linkage |
|--------|--------|----------|-----------------|
| CCC Score | Highest | Moderate | Moderate |
| Segment Balance | Good | Uneven | Moderate |
| Cluster Separation | Strong | Moderate | Good |
| Recommended | Yes | No | No |

---

## How to Replicate

Prerequisites:
- SAS Enterprise Miner v15.2 or higher
- Dataset: Heart Disease.sas7bdat
- Diagram file: GK.Cluster Analysis.xml

Steps:
1. git clone https://github.com/mgkgopikrishna/heart-disease-clustering.git
2. Open SAS Enterprise Miner, create project: Heart_Disease_Analytics
3. Diagrams -> Import Diagram -> select GK.Cluster Analysis.xml
4. Import Heart Disease.sas7bdat into project library
5. Connect data source to pipeline
6. Right-click Reporter/Cluster node -> Run -> Results

---

## Key Lessons

- Unsupervised learning surfaces clinically meaningful groups with no predefined labels
- STD normalization is critical — without it, BMI dominates binary features
- Ward's method outperforms centroid approaches for complex clinical data
- CCC-based automatic selection removes bias from choosing cluster count
- Three segments is clinically optimal — actionable without over-segmenting

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| SAS Enterprise Miner v15.2 | Pipeline and clustering |
| Ward's Hierarchical Clustering | Primary algorithm |
| Centroid + Average Linkage | Comparison methods |
| Cubic Clustering Criterion | Optimal cluster selection |
| STD Normalization | Feature scaling |

---

## Built By

Gopi Krishna Marka — MLOps Engineer | Data Scientist | Cloud Engineer

Applying data engineering and machine learning to real-world clinical datasets
