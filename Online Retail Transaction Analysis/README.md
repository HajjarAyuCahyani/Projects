# 🛒 Online Retail Transaction Analysis
### EDA · RFM Segmentation · Market Basket Analysis · Time Series

---

## 🧩 The Business Problem

Imagine you're running an online retail store based in the UK that ships to customers across Europe. You have over **500,000 transaction records** — but you don't really know:

- Who your best customers are (and who's quietly drifting away)
- Which products are secretly dragging down your profit
- What products customers tend to buy *together* (and how to use that to sell more)
- When your store is actually busiest — and when it's dead

This project answers all of that using real transaction data, turning a messy spreadsheet into a clear picture of the business.

---

## 📦 Dataset

| Detail | Info |
|---|---|
| Source | [Kaggle — Online Retail Transaction Data](https://www.kaggle.com/datasets/thedevastator/online-retail-transaction-data) |
| Size | 500,000+ rows |
| Period | December 2010 – December 2011 |
| Coverage | UK + international customers across Europe |
| Format | Excel (.xlsx) |

---

## 🔍 What I Found — The Story in the Data

### Chapter 1: Not All Transactions Are Sales

Before any analysis, the first question was: *what exactly is in this data?*

Turns out, not everything is a clean sales record. After classifying every row by transaction logic (quantity sign × price sign × invoice prefix), the data breaks down into **5 distinct business types**:

| Type | Count | Business Meaning |
|---|---|---|
| **Sales** | 524,878 | Core revenue — healthy |
| **Returns** | 9,251 | Profit leakage — needs QC review |
| **Damage / Write-off** | 1,336 | Warehouse loss — needs SOP audit |
| **Free Items / Promo** | 1,174 | Marketing spend — ROI unclear |
| **Adjustment / Error** | 2 | Accounting correction — safe to ignore |

> **Key insight:** 87%+ of transactions are genuine sales. But the 9,000+ returns and 1,300+ write-offs represent silent profit drains that ops and QC teams should investigate.

---

### Chapter 2: Who Are the Customers, Really?

Descriptive stats on purchase quantity revealed a sharp split:

- **Median order: 3 items** → The typical customer is a casual retail buyer (B2C)
- **Max order: 80,000 units** → A small group of wholesale buyers (B2B) exists and skews the data massively

This distinction matters: **the same pricing and marketing strategy won't work for both segments.**

Before modeling, extreme wholesale outliers were removed using an **IQR × 3 threshold** to preserve the B2C signal without distorting the analysis.

---

### Chapter 3: RFM Segmentation — Ranking Every Customer

With clean data in hand, every customer was scored on three dimensions:

- **Recency (R):** How recently did they buy?
- **Frequency (F):** How often do they buy?
- **Monetary (M):** How much have they spent in total?

Scoring used **quantile-based ranking (pd.qcut)** — not mean-based cuts — because customer spending follows a highly skewed distribution. Mean-based scoring would have unfairly labelled 90% of customers as low-value.

The result: **5 customer profiles**

| Segment | Profile | Recommended Action |
|---|---|---|
| **Champions** | High R, F, M — loyal and recent | VIP loyalty program |
| **Loyal Customers** | Frequent buyers with solid spend | Early access, exclusive offers |
| **Potential Loyalists** | Recent but not yet frequent | Trigger second purchase incentive |
| **New Customers** | Just arrived, small basket | Onboarding sequence, first offer |
| **Hibernating / Lost** | Long gone, low engagement | Win-back campaign or let churn |

> **Strategic implication:** Not every customer deserves the same marketing budget. Champions and Loyal Customers generate disproportionate revenue — concentrate retention effort there first.

---

### Chapter 4: Market Basket Analysis — What Gets Bought Together?

Using the **FP-Growth algorithm** on UK transactions (the dominant market at ~90% of data), the analysis uncovered product association rules — which items customers tend to buy in the same basket.

**Why FP-Growth over Apriori?**  
The transaction matrix here has thousands of unique products. FP-Growth uses a compressed tree structure that's significantly faster and more memory-efficient at this scale.

**Filters applied:**
- Minimum support: 2% (appears in at least 2% of baskets)
- Minimum lift: >1 (genuinely correlated, not coincidental)

The output: a ranked list of product pairs sorted by **lift** — the strongest signal of true purchasing association beyond random chance.

> **Business application:** Product bundling, "frequently bought together" recommendations, and cross-sell placement on product pages. This is the data behind those "customers also bought" sections on e-commerce sites.

---

### Chapter 5: Time Patterns — When Does the Store Actually Run?

Three operational insights from time series analysis:

**Monthly Revenue Trend (Dec 2010 – Nov 2011)**
- Revenue peaks sharply in **October–November** — classic pre-Christmas surge
- January sees a significant dip — post-holiday correction
- Note: December 2011 data is incomplete and excluded to avoid misleading the trend line

**Busiest Days**
- **Thursday** leads in transaction volume
- **Saturday has zero transactions** — the store appears to be closed or non-operational on Saturdays (notable for inventory and staffing planning)

**Peak Hours (Rush Hour)**
- Transaction volume surges between **10 AM – 2 PM**
- Activity drops sharply after 3 PM
- Virtually no transactions before 7 AM or after 7 PM

> **Operational implication:** Staff scheduling, flash sale timing, and marketing campaign launches should align with Thursday mornings — not random days or off-peak hours.

---

## 🛠️ Tools & Methods

| Category | Tools / Methods |
|---|---|
| Language | Python |
| Data Wrangling | Pandas |
| Visualization | Matplotlib, Seaborn |
| Market Basket | mlxtend (FP-Growth, Association Rules) |
| Segmentation | RFM with quantile scoring (pd.qcut) |
| Outlier Handling | IQR × 3 method |
| Export | CSV → Tableau for dashboard |

---

## 📁 Repository Structure

```
├── Online_Retail_Analysis.ipynb   # Full analysis notebook
├── df_produk.csv                  # Cleaned product transaction data
├── rfm_segmentation.csv           # RFM scores per customer
├── market_basket_rules_uk.csv     # Association rules output
└── README.md
```

---

## 💡 Key Takeaways

1. **Raw transaction data is never clean** — classifying records by business type before analysis is not optional, it's essential.
2. **Skewed data requires rank-based scoring** — mean-based RFM cuts would misclassify the majority of customers.
3. **B2C and B2B segments need separate strategies** — one pricing or marketing approach does not fit both.
4. **Market basket rules are only as good as your support threshold** — too high and you miss niche associations, too low and you drown in noise.
5. **Operational patterns (rush hours, dead days) are hidden in timestamp data** — and most businesses don't look there.

---

## 🚀 Possible Next Steps

- **Customer churn prediction model** — use RFM scores as features to predict which Potential Loyalists will go Hibernating next
- **Cohort retention analysis** — track whether new customers acquired in Q1 are still active in Q4
- **Tableau dashboard** — visualize RFM segments, monthly trends, and basket rules interactively

---

*Independent project using publicly available dataset from Kaggle.*  
*All insights are exploratory and based on the available data period (Dec 2010 – Nov 2011).*
