# Heart Disease Predictive Analytics & Unsupervised Clustering Pipeline


An end-to-end data engineering and predictive modeling project analyzing risk factors for cardiovascular disease. This project utilizes a comprehensive clinical dataset to identify distinct patient risk segments using advanced unsupervised clustering methodologies in **SAS Enterprise Miner**, supported by exploratory data analysis.

---

## 📊 Project Architecture Overview
The data pipeline ingests raw clinical metrics, processes them through an observation filtering stage, and branches into multiple parallel clustering configurations to evaluate structural similarities in patient risk groups.

### Process Flow Diagram
![SAS Enterprise Miner Process Flow Diagram](process_flow_diagram.png)
> *Guidance: Take a screenshot of your SAS Enterprise Miner workspace window showing the nodes connected from the Data Source -> Filter -> Clustering and save it as `process_flow_diagram.png` in your repository root.*

---

## 🛠️ Key Technical Features & Stack
* **Analytics Engine:** SAS Enterprise Miner (v15.2)
* **Data Dimensions:** 29,944 historical patient observations across 18 clinical attributes (BMI, Smoking, Alcohol Consumption, Diabetic Status, Physical/Mental Health indexes, etc.).
* **Data Preprocessing:** Outlier elimination via statistical metadata filters.
* **Algorithms Evaluated:**
    * Ward's Hierarchical Clustering Method (Minimum Variance Criterion)
    * Centroid Clustering Optimization
    * Average Linkage Clustering Strategy

---

## 📈 Detailed Step-by-Step Implementation

### Step 1: Data Ingestion & Metadata Mapping
* The raw dataset `Heart Disease.sas7bdat` containing 29,944 rows is loaded via an Input Data Source node.
* Variables are automatically mapped into categorical (nominal/binary) and continuous (interval) scales. Key parameters include:
    * `Target Role`: Structural mapping for classification tracking.
    * `Measurement Levels`: Binary flags for behavioral inputs (Smoking, Alcohol Drinking, Physical Activity).

### Step 2: Data Cleaning & Statistical Filtering
* To prevent extreme clinical values or unrepresentative data from skewing the clusters, a **Filter Node** is introduced.
* Observations falling outside normal distribution boundaries are flagged and excluded from downstream model training to ensure tighter cluster definitions.

### Step 3: Cluster Analysis & Segmentation Setup
Parallel structural groupings are executed using the **Cluster Node** to segment patient populations:
* **Normalization Strategy:** Inputs are standardized using the `STD` (Standard Deviation) method to prevent high-magnitude continuous fields (like `BMI`) from dominating binary indicators.
* **Clustering Method:** Configured primarily on **Ward's Minimum Variance Method** to minimize the total within-cluster variance.
* **Segment Selection:** Set to `AUTOMATIC` with a maximum cutoff threshold to naturally discover optimal risk groups based on the Cubic Clustering Criterion (CCC).

### Step 4: Segment Profile Analysis
![SAS Enterprise Miner Segment Plot](segment_plot.png)
> *Guidance: Open your Segment Plot from the SAS results panel showing the distribution histograms of AgeCategory, Race, and Diabetic status across segments, save it as `segment_plot.png`, and upload it.*

---

## 🔬 Core Insights & Analytical Results

Based on the final generated cluster report (`REPORT 123.pdf`), the pipeline successfully segmented patients into distinct actionable profiles:

| Segment ID | Key Clinical Identifiers | Primary Demographics | Dominant Risk Classification |
| :--- | :--- | :--- | :--- |
| **Segment 1** | High physical activity, low BMI, excellent self-reported health. | Multi-age distribution, non-smokers. | **Low Risk Control Group** |
| **Segment 2** | High prevalence of Difficult Walking, poor physical health metrics. | Heavily concentrated in the `80 or older` demographic. | **High Age-Related Risk** |
| **Segment 3** | High continuous BMI metrics (26.5 - 31.0), borderline or confirmed diabetes. | Dominantly `White` demographic blocks. | **Metabolic & Lifestyle Risk** |

---

## 🚀 How to Replicate This Project

### Prerequisites
* SAS Enterprise Miner (v15.2 or higher)
* The project dataset: `Project Data Set Modified (8).xlsx` or converted `.sas7bdat` file.

### Execution Steps
1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/gopikrishna88/heart-disease-clustering.git](https://github.com/gopikrishna88/heart-disease-clustering.git)
    cd heart-disease-clustering
    ```
2.  **Import XML Diagram Blueprint:**
    * Open SAS Enterprise Miner.
    * Create a new project named `Heart_Disease_Analytics`.
    * Right-click on **Diagrams** -> **Import Diagram** and select the `GK.Cluster Analysis.xml` file included in this repository.
3.  **Link the Data Source:**
    * Import `Heart Disease.sas7bdat` into your project library.
    * Drag the data source onto the workspace and connect it directly to the first node.
4.  **Run the Pipeline:**
    * Right-click the final **Reporter** or **Cluster** node in the diagram tree.
    * Select **Run**. Once complete, click **Results** to view the full statistical breakdown.
