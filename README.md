# 🇨🇦 Ontario–US Trade Dashboard ( OPS HACKATHON 2025 )

Comprehensive Data Ingestion, Analytics, Machine Learning & Reporting
**With full Microsoft Fabric integration guide**

---

## 📌 Overview

This repository contains a full pipeline for analyzing Ontario–US trade data, including:

* **Data ingestion** notebooks for HS2, NAPCS, monthly import/export data
* **Transformation & cleaning** workflows
* **Machine learning** models (Random Forest + CleanLab classification improvement)
* **Reports & dashboards** (`.pbix` Power BI report included)
* **Environment files** (`conda.yaml`, `requirements.txt`, `python_env.yaml`)
* **ML model folder** (`MLmodel/`)

This guide explains:

1. How the project is structured
2. How you can run it locally
3. **How to use the entire project inside Microsoft Fabric**, including Git integration, notebooks, pipelines, ML, and dashboards.

---

# 🗂 Folder Structure

```
Ontario-US-Trade-Dashboard/
│
├── data/                         # Raw / input data (if included)
├── MLmodel/                      # Trained model artifacts
│   ├── random_forest_model.pkl
│   └── cleanlab_output.csv
│
├── notebooks/
│   ├── nb_hs2_ontario_us_trade_data_ingestion.ipynb
│   ├── nb_monthly_ontario_import_export_ingestion.ipynb
│   ├── nb_napcs_trade_data_ingestion.ipynb
│   ├── nb_helper_tables.ipynb
│   └── Machine Learning.ipynb
│
├── reporting/
│   ├── rpt_ontario_us_trade_analytics.pbix
│   └── rpt_ontario_us_trade_analytics.pdf
│
├── environment/
│   ├── conda.yaml
│   ├── python_env.yaml
│   └── requirements.txt
│
└── README.md
```

---

# ⚙️ Local Setup (Optional)

## 1. Clone the repository

```
git clone https://github.com/VNBcoding/Ontario-US-Trade-Dashboard.git
cd Ontario-US-Trade-Dashboard
```

## 2. Create environment

Using Conda:

```
conda env create -f environment/conda.yaml
conda activate ontario-trade-env
```

Or using requirements:

```
pip install -r environment/requirements.txt
```

## 3. Run notebooks

Open VS Code or Jupyter Lab and execute notebooks inside the `/notebooks` folder.

---

## Using This Project in Microsoft Fabric

This section teaches users how to run **your exact codebase inside Microsoft Fabric** using Git integration, notebooks, pipelines, and dashboards.

---

## 1. Create a Microsoft Fabric Workspace

1. Go to **[https://app.fabric.microsoft.com](https://app.fabric.microsoft.com)**
2. Create a **new workspace**
3. Ensure the workspace has **Fabric Capacity enabled**

   * Trial
   * Fabric Premium (P1/P2/etc.)
   * Or your organization’s Fabric capacity

---

## 2. Connect the Workspace to Your GitHub Repo

Fabric supports full Git sync.

1. In Fabric → open your workspace
2. Click:
   **Workspace Settings → Git Integration**
3. Choose:
   **GitHub → Connect**
4. Enter your GitHub repo URL:

```
https://github.com/VNBcoding/Ontario-US-Trade-Dashboard
```

5. Select branch (usually `main`)
6. Click **Apply → Sync**

Fabric will now import notebooks, folders, and supported items from the repo into your workspace.

---

# 📥 3. Run Data Ingestion Notebooks in Fabric

Open:
**Workspace → Notebooks → (choose ingestion notebook)**
Examples:

* `nb_hs2_ontario_us_trade_data_ingestion.ipynb`
* `nb_napcs_trade_data_ingestion.ipynb`
* `nb_monthly_ontario_import_export_ingestion.ipynb`

### ⚠️ IMPORTANT

Modify file paths so they save into **Fabric Lakehouse**, not local disk.

Use:

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

df = spark.read.csv("Files/your-data.csv", header=True)
df.write.format("delta").mode("overwrite").save("Tables/ontario_trade")
```

Fabric storage options:

| Fabric Storage | Recommended Path    |
| -------------- | ------------------- |
| Lakehouse      | `Tables/...`        |
| Files          | `Files/...`         |
| Data Warehouse | SQL ingestion later |

After running ingestion notebooks:

✔ Data will exist inside Fabric
✔ Dashboards & ML can connect to it

---

# 🤖 4. Run Machine Learning Notebook in Fabric

Open:

```
Machine Learning.ipynb
```

Fabric supports Python runtime for:

* Pandas
* NumPy
* Scikit-learn
* CleanLab
* Spark

If the notebook loads MLmodel files, upload them to:
**Workspace → Lakehouse → Files/MLmodel**

Modify the load path:

```python
model_path = "/lakehouse/default/Files/MLmodel/random_forest_model.pkl"
```

After running the ML notebook, save predictions back to Fabric:

```python
predictions_df.write.format("delta").mode("overwrite").save("Tables/predictions")
```

---

# 📊 5. Use Power BI / Build Fabric Dashboards

You have **two options**:

---

## 🅰️ Option A — Build Dashboards Directly in Fabric (Recommended)

1. Go to **Real-Time Analytics / Lakehouse / SQL endpoint**

2. Connect tables such as:

   * `ontario_trade`
   * `napcs_trade`
   * `monthly_import_export`
   * `predictions`

3. Create:

   * Dashboards
   * Dataflows
   * Semantic Models
   * Visuals

Fabric dashboards automatically refresh when Lakehouse data updates.

---

## 🅱️ Option B — Import the Existing Power BI Report (`pbix`)

Open:

```
reporting/rpt_ontario_us_trade_analytics.pbix
```

Inside Power BI Desktop:

1. Go to **Transform Data → Data Source Settings**
2. Change source to Fabric Lakehouse:

```
"https://onelake.dfs.fabric.microsoft.com/yourworkspace/Lakehouses/yourlakehouse/Files/..."
```

3. Save & Publish to Fabric workspace.

---

# 🔁 6. Maintaining Git Version Control Inside Fabric

Fabric supports full Git workflows:

* When you modify notebooks, pipelines, dashboards →
  **Use Fabric Source Control panel to Commit & Push**
* Developers can clone the repo locally
* Team members can branch, merge, and collaborate normally

Every Fabric item stays in sync with GitHub.

---

# 🚀 End-to-End Workflow Summary

| Step                          | Tool                         | Output                              |
| ----------------------------- | ---------------------------- | ----------------------------------- |
| Ingest HS2/NAPCS/Monthly data | Fabric Notebook/Pipeline     | Delta Tables in Lakehouse           |
| Clean / Transform             | Spark / Pandas               | Structured tables                   |
| Machine Learning              | ML Notebook + model folder   | Predictions table                   |
| Reporting                     | Fabric Dashboard or Power BI | Ontario–US Trade Insights Dashboard |
| Version control               | GitHub + Fabric Git Sync     | Fully reproducible project          |

---

# Contributing

Pull requests are welcome.

For major changes, please open an issue first to discuss what you'd like to modify.

