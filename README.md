# Marketing Campaigns Analysis
### Course-End Project — Applied Data Science with Python

**Author:** Ajaykanna E | **Course:** Applied Data Science with Python  
**GitHub:** https://github.com/AjaykannaE/Marketing-campaigns-analysis

---

## Project Overview

This project takes the role of a **Data Scientist** analyzing a marketing dataset to understand customer behavior, evaluate campaign effectiveness, and generate actionable business insights using **Exploratory Data Analysis (EDA)** and **Hypothesis Testing**.

The analysis is structured around the **Marketing Mix (4 Ps)** framework — Product, Price, Place, and Promotion — examining how customer demographics, spending behavior, and campaign responses interact.

---

## Dataset

**File:** `marketing_data.csv` — 2,240 customer records with 28 columns

| Column | Description |
|--------|-------------|
| `Income` | Annual customer income (currency formatted) |
| `Dt_Customer` | Date of customer enrollment |
| `Education` | Education level |
| `Marital_Status` | Marital status |
| `Kidhome` / `Teenhome` | Number of kids / teens at home |
| `MntWines` to `MntGoldProds` | Spending on each product category |
| `NumWebPurchases` / `NumCatalogPurchases` / `NumStorePurchases` | Purchases by channel |
| `AcceptedCmp1–5` | Whether customer accepted each of the 5 campaigns |
| `Response` | Acceptance of the most recent (last) campaign |
| `Complain` | Whether customer complained in the last 2 years |
| `Country` | Customer's country |

---

## Project Structure

```
├── Marketing_Campaigns_Project_AjaykannE.ipynb   # Main notebook
├── marketing_data.csv                             # Marketing dataset
└── README.md
```

---

## Steps Covered

### Step 1 — Data Loading & Inspection
- Loaded 2,240 rows × 28 columns
- Identified `Income` stored as string (due to `$` and `,` formatting) and `Dt_Customer` stored as object — both needing type conversion

### Step 2 — Data Cleaning & Missing Value Imputation
- Stripped currency symbols and whitespace from `Income`; cast to float
- Converted `Dt_Customer` to proper datetime
- Standardized `Education`: mapped `'2n Cycle'` → `'Master'`
- Cleaned `Marital_Status`: mapped `'YOLO'`, `'Alone'`, `'Absurd'` → `'Single'`
- Imputed **24 missing Income values** using **group-level median** (grouped by Education + Marital Status) for accurate, context-aware fill

### Step 3 — Feature Engineering
Derived four new analytical features:

| Feature | Description |
|---------|-------------|
| `Total_Children` | Sum of `Kidhome` + `Teenhome` |
| `Age` | Calculated from `Year_Birth` using reference year 2014 |
| `Total_Spending` | Sum of all 6 product expenditure columns |
| `Total_Purchases` | Sum of web + catalog + store purchases |

### Step 4 — Distribution Analysis & Outlier Treatment
- Generated histograms and box plots for 10 key numeric features
- Found right-skewed distributions in Income, spending columns, and age anomalies (implausible birth years)
- Applied **IQR-based capping (Winsorization)** — outliers clipped to 1.5×IQR fences rather than dropped, preserving data volume

### Step 5 — Encoding Categorical Variables
- **Ordinal Encoding** for `Education` (Basic=0 → PhD=3) — respects natural hierarchy
- **One-Hot Encoding** for `Marital_Status` and `Country` — prevents false ordinal assumptions

### Step 6 — Correlation Heatmap
Key findings from the Pearson correlation matrix:

| Relationship | Insight |
|-------------|---------|
| Income ↔ Total_Spending | Strong positive — higher earners spend significantly more |
| Total_Children ↔ Total_Spending | Moderate negative — more children = less discretionary spend |
| NumStorePurchases ↔ NumCatalogPurchases | Positive — offline channel users tend to use both |
| Campaign acceptance (Cmp1–5, Response) | Moderate mutual correlation — campaign-responsive customers accept multiple campaigns |

### Step 7 — Hypothesis Testing
Four business hypotheses tested at **α = 0.05**:

| # | Hypothesis | Test Used |
|---|-----------|-----------|
| H1 | Older customers prefer in-store shopping | Pearson Correlation (Age vs NumStorePurchases) |
| H2 | Customers with children prefer online shopping | Welch's Independent t-test (Web purchases: parents vs non-parents) |
| H3 | In-store sales are cannibalized by web/catalog channels | Pearson Correlation (Store vs Web, Store vs Catalog) |
| H4 | USA significantly outperforms other countries in total purchases | Welch's Independent t-test (US vs non-US Total_Purchases) |

Each hypothesis includes H₀/H₁ statements, p-value interpretation, and visualization.

### Step 8 — Business Insights through Visualization
Five targeted business questions answered:

**8a. Top & Lowest Revenue Products**
- **Wines** is the dominant product category by a significant margin
- **Fruits** and **Sweets** are the lowest revenue generators — candidates for targeted promotions

**8b. Age vs Last Campaign Acceptance Rate**
- Campaign acceptance rates segmented across age groups (Under 35 / 35–50 / 50–65 / Above 65)
- Point-biserial correlation used to quantify the relationship

**8c. Country with Highest Campaign Acceptance**
- Country-level acceptance rates ranked and visualized
- Identifies geographies where campaigns resonate most for replication

**8d. Number of Children vs Total Expenditure**
- Customers with **zero children** spend significantly more
- Spending declines progressively with number of children — child-free customers are the highest-value segment

**8e. Education Background of Complainers**
- Analyzed complaint rates across education levels
- Helps customer service tailor resolution strategies by education segment

---

## Key Business Insights Summary

| Area | Insight |
|------|---------|
| **Top Product** | Wines dominates revenue; Fruits & Sweets underperform |
| **Customer Value** | Child-free, high-income customers are the highest spenders |
| **Channels** | Store and catalog channels complement each other (not cannibalistic) |
| **Campaigns** | Campaign-responsive customers tend to accept multiple campaigns |
| **Geography** | Campaign effectiveness varies significantly by country |

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python 3 |
| Data Manipulation | pandas, numpy |
| Visualization | matplotlib, seaborn |
| Statistical Testing | scipy (pearsonr, ttest_ind, f_oneway, pointbiserialr) |
| Encoding | scikit-learn (LabelEncoder, OrdinalEncoder), pandas get_dummies |

---

## How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/AjaykannaE/Marketing-campaigns-analysis.git
   cd Marketing-campaigns-analysis
   ```

2. **Install dependencies**
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn
   ```

3. **Place the dataset** (`marketing_data.csv`) in the same directory as the notebook.

4. **Run the notebook**
   ```bash
   jupyter notebook Marketing_Campaigns_Project_AjaykannE.ipynb
   ```
   Run all cells from top to bottom.

---

## License

This project is open-source and available under the [MIT License](LICENSE).
