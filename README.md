# 🧠 Data Warehousing & Mining – ETL Project (DSA 2040A FS 2025)

> 🧩 *An end-to-end Python ETL pipeline transforming raw retail sales data into clean, analytics-ready insights.*

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-green?logo=pandas)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange?logo=plotly)
![SQLite](https://img.shields.io/badge/SQLite-Database-lightblue?logo=sqlite)
![GitHub](https://img.shields.io/badge/Platform-GitHub-black?logo=github)
![License](https://img.shields.io/badge/License-Academic-lightgrey)

---

## 🗂️ Project Title
### **Sales Transactions ETL Pipeline Using Python & Pandas**  

A complete Extract–Transform–Load workflow applied to real-world retail data from Kaggle’s *Superstore Sales* dataset.  
This project demonstrates data extraction, quality assessment, transformation, enrichment, loading, and visualization in preparation for analytical use.

---

## 🧾 1️⃣ Project Overview
The objective of this project was to design and execute the **Extract**, **Transform**, and **Load** phases of an ETL pipeline.  
The workflow loads, inspects, cleans, standardizes, enriches, and stores a dataset containing retail sales transactions, preparing it for future analysis or data warehousing.

### ✅ Key Goals:
- Load and inspect large-scale retail data  
- Detect and document data quality issues  
- Apply diverse transformations across multiple categories  
- Load structured data into SQLite/Parquet formats  
- Generate clean, analytics-ready datasets  

---

## 💾 2️⃣ Dataset Description

**📍 Source:** [Superstore Sales Dataset – Kaggle](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)  
**📊 Size:** ~10,000 rows × 21 columns  
**📂 Format:** CSV (`Raw_data.csv`)

**Main Features:**
| Category | Example Columns | Description |
|-----------|-----------------|--------------|
| **Order Info** | `Order ID`, `Order Date`, `Ship Date`, `Ship Mode` | Purchase and shipping details |
| **Customer Info** | `Customer ID`, `Customer Name`, `Segment`, `Region` | Customer demographics |
| **Product Info** | `Product ID`, `Category`, `Sub-Category`, `Product Name` | Product classification |
| **Sales Metrics** | `Sales`, `Quantity`, `Discount`, `Profit` | Transactional performance metrics |

---

## ⚙️ 3️⃣ Tools & Technologies

| Tool | Purpose |
|------|----------|
| 🐍 **Python (Pandas, NumPy)** | Data manipulation and transformation |
| 📊 **Matplotlib** | Data visualization and insights |
| 🧮 **Jupyter Notebook** | Interactive documentation and reproducibility |
| 🗄️ **SQLite / Parquet** | Data loading and persistence |
| 💻 **Git & GitHub** | Version control and public submission |

---

## 🧭 4️⃣ Project Folder Structure

ET_Exam_Peter_341/
├── data/
│ ├── raw_data.csv
│ ├── incremental_data.csv
│ ├── validated_full.csv
├── transformed/
│ ├── transformed_full.csv
│ ├── transformed_incremental.csv
├── loaded/
│ ├── full_data.db
│ ├── full_data.parquet
│ ├── incremental_data.parquet
├── etl_extract.ipynb
├── etl_transform.ipynb
├── etl_load.ipynb
├── README.md
└── .gitignore

---

## 🔍 5️⃣ ETL Workflow Summary

### **A. 🧮 Extract Phase (`etl_extract.ipynb`)**
**Steps Performed:**
1. Loaded `raw_data.csv` and `incremental_data.csv` using Pandas.  
2. Conducted exploratory profiling using `.head()`, `.info()`, and `.describe()`.  
3. Checked for:
   - ✅ Missing values → None found  
   - ✅ Duplicates → None found  
   - ✅ Invalid numerical values → None found  
4. Verified proper column data types (dates, numbers, text).  
5. Merged datasets and saved validated copy as `validated_full.csv`.

**Result:**  
> Dataset was clean and consistent — ready for transformation.

---

### **B. 🔧 Transform Phase (`etl_transform.ipynb`)**

Applied **7 major transformations** across multiple categories:

| # | Transformation | Category | Description |
|---|----------------|-----------|--------------|
| 1️⃣ | Convert `Order Date` & `Ship Date` → datetime | Standardization | Enables date operations |
| 2️⃣ | Create `Shipping_Duration_days` | Enrichment | Days between order and delivery |
| 3️⃣ | Compute `Profit_Margin = Profit / Sales` | Enrichment | Adds business profitability metric |
| 4️⃣ | Convert `Postal Code` → string | Structural | Preserves leading zeros |
| 5️⃣ | Clean text fields (trim spaces) | Cleaning | Improves consistency |
| 6️⃣ | Create `Sales_Tier` (Low/Medium/High) | Categorization | Sales segmentation using quantiles |
| 7️⃣ | Derive `Revenue_per_Item = Sales / Quantity` | Enrichment | Per-item revenue insight |

**Files Generated:**
- 📄 `transformed_full.csv` — Transformed main dataset  
- 📄 `transformed_incremental.csv` — Transformed incremental dataset  

---

## 📈 6️⃣ Visualizations & Insights

### **1️⃣ Total Sales & Profit by Region**
```python
trans_raw.groupby('Region')[['Sales','Profit']].sum().plot(kind='bar')
🧱 7️⃣ Load & Verification

The Load Phase marks the final step of the ETL pipeline, where the transformed datasets were successfully stored for long-term use and analysis.

🗄️ Load Formats Used

SQLite Database: Stored datasets in Loaded/full_data.db using sqlite3.

Parquet Files: Also exported to Loaded/full_data.parquet for lightweight, fast storage.

🔍 Verification

SQLite verification was performed using a simple SQL query:

import sqlite3, pandas as pd

with sqlite3.connect('Loaded/full_data.db') as conn:
    preview = pd.read_sql('SELECT * FROM full_data LIMIT 5;', conn)
display(preview)


Sample Output:

Row ID	Order ID	Order Date	Ship Date	Sales	Profit	Sales_Tier
1	CA-2016-152156	2016-11-08	2016-11-11	261.96	41.91	Medium
2	CA-2016-152156	2016-11-08	2016-11-11	731.94	219.58	High

Record count verification:

SELECT COUNT(*) FROM full_data;
→ 8996

⚙️ Issues & Resolutions
Issue	Resolution
❌ Database connection closed early	✅ Moved verification query before conn.close()
❌ Missing folder “Loaded”	✅ Created directory using os.makedirs()
❌ Parquet write error	✅ Installed dependency pyarrow
⚠️ Datatype mismatch warning	✅ Adjusted column types before saving

✅ Both SQLite and Parquet formats loaded and verified successfully.

🧰 8️⃣ How to Run the Project
Requirements
pip install pandas numpy matplotlib jupyter pyarrow

Execution Steps

Open Jupyter Notebook.

Run etl_extract.ipynb → performs extraction and profiling.

Run etl_transform.ipynb → applies all transformations.

Run etl_load.ipynb → loads data into SQLite and Parquet.

(Optional) Run visualization cells for insights.

All notebooks are fully re-runnable and require no manual edits.

💡 9️⃣ Key Learnings

Understanding full ETL workflow (Extract → Transform → Load).

Performing data profiling, validation, and standardization.

Applying enrichment and categorization transformations.

Storing datasets in both SQLite and Parquet formats.

Using Python and Pandas for automation and reproducibility.

Managing version control and documentation via GitHub.

🏁 🔟 Conclusion

This project successfully demonstrates an end-to-end ETL pipeline using Python and Pandas.
Data was extracted, cleaned, transformed, loaded, and validated into efficient storage formats.
The final dataset is standardized, enriched, and analytics-ready — providing insights into sales performance, profitability, and logistics efficiency.

The final output serves as a strong foundation for future Load, OLAP, or business intelligence applications.
