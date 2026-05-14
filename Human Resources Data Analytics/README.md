# 👥 Human Resources Data Analytics
### Attrition Risk · Behavioral Anomaly Detection · Leadership Effectiveness · Recruitment ROI

---

## 🧩 The Business Problem

Every company has the same blind spots: they know *that* employees are leaving, but not *why* or *who* is really the problem. They know salaries are going up, but not whether the investment is actually paying off. They know managers vary in style, but not whether that variance is creating systemic unfairness.

This project treats HR data the way a business analyst would — not as a people database, but as an operational dataset full of signals about where the organization is leaking value.

Three core questions drive the analysis:

1. **Are we spending recruitment budget wisely?** Which channels deliver the best performance-per-dollar?
2. **Where are the behavioral inefficiencies?** Who is being overpaid for underperformance — and what does the data say about engagement and discipline?
3. **Is leadership fair and structurally sound?** Are managers scoring employees consistently, and are any carrying unsustainable cross-departmental loads?

---

## 📦 Dataset

| Detail | Info |
|---|---|
| Source | [Kaggle — HR Dataset v14](https://www.kaggle.com/datasets/rhuebner/human-resources-data-set) |
| Records | 311 employees |
| Features | 36 columns covering demographics, performance, engagement, sourcing, and tenure |
| Format | CSV |

---

## 🔍 What I Found — The Story in the Data

### Chapter 1: The Data Had a Hidden Problem

Before any analysis, a structural data issue surfaced: **76% of one manager's team (Webster Butler) had missing ManagerID values.**

Rather than dropping or ignoring these rows, targeted imputation assigned the correct ManagerID (39) based on the `ManagerName` column. This preserved 100% of the data while maintaining relational integrity — critical for any downstream leadership analysis.

> **Why this matters:** Blindly dropping missing values here would have made Webster Butler's team invisible in all manager-level analyses — a silent bias baked into the dataset.

---

### Chapter 2: Salary Tells One Story. Engagement Tells Another.

A Spearman correlation analysis across key variables revealed two non-obvious findings:

**1. Salary does not drive satisfaction.**
There is no strong correlation between how much employees earn and how satisfied they report being. Non-monetary factors — recognition, growth, environment — likely play a larger role.

**2. Engagement predicts punctuality more than satisfaction does.**
Employees with lower engagement scores show higher tardiness. This means: improving benefits won't fix a punctuality problem. What will? Improving the employee's emotional connection to their work.

> **Implication for HR:** "Satisfaction programs" (perks, salary bumps) and "engagement programs" (purpose, manager quality, recognition) target different problems and shouldn't be confused.

---

### Chapter 3: The Overpaid Underperformers

To investigate payroll efficiency, employees were first segmented into three job tiers: **Executive, Management, and Staff.** This prevents executive-level salaries from skewing comparisons across the workforce.

Within each tier, an "Anomaly" was defined as an employee who meets all three of the following conditions:

- ✅ **Earns above their tier's average salary**
- ✅ **Has zero special projects assigned**
- ✅ **Has recorded tardiness in the last 30 days**

The result: a concrete, data-driven shortlist of employees where salary investment is not aligned with contribution or discipline — giving HR a factual basis for performance review conversations rather than gut feeling.

> **Why three criteria?** Any single criterion alone would flag false positives. High earners might be senior specialists. Zero-project employees might be in operational roles. Combining all three isolates a genuinely problematic pattern.

---

### Chapter 4: When Do Employees Actually Leave?

Using KDE (Kernel Density Estimation) on employee tenure, the resignation pattern shows a clear **bimodal distribution:**

- **Peak 1 — Year 1 to 2:** High early turnover, likely driven by poor role-fit or onboarding failure
- **Stabilization — Year 3 onwards:** Employees who survive the first 3 years tend to become loyal, long-term contributors

This is not a random scatter of resignations across all tenure lengths. It's a structured pattern — which means it's addressable.

| Tenure Band | Resignation Risk | Recommended Intervention |
|---|---|---|
| 0 – 2 years | **High** | Structured onboarding, 30/60/90-day check-ins |
| 2 – 3 years | **Moderate** | Career path conversation, engagement survey |
| 3+ years | **Low** | Retention milestone recognition, growth opportunities |

---

### Chapter 5: Is the Recruitment Budget Going to the Right Channels?

Not all recruitment channels are equal — but are the differences real or just noise?

**Step 1: Calculate Efficiency Score per channel**

$$\text{Efficiency Score} = \frac{\text{Average Performance Score}}{\text{Average Salary}} \times 100{,}000$$

This gives a performance-per-dollar metric that makes channels directly comparable.

**Step 2: Statistical validation (Kruskal-Wallis H-test)**

Before acting on efficiency scores, the test confirmed whether performance differences across channels are statistically significant.

**Result: P-value = 0.70 — Not significant.**

Performance is statistically identical across all recruitment channels. This changes the decision entirely:

| Channel | Avg Salary | Performance | Efficiency | Verdict |
|---|---|---|---|---|
| Website / Google Search | Lower | Equal | **Highest ROI** | ✅ Scale up |
| LinkedIn | Mid | Equal | Moderate | ✅ Stable |
| Employee Referral | Higher (+15–20%) | Equal | Lower | ⚠️ Re-evaluate |
| Indeed | Higher | Equal | Lowest | ❌ Reduce spend |

> **The "premium" trap:** Referral hires and Indeed applicants cost significantly more in salary — but deliver statistically identical performance. The data recommends optimizing SEO and career pages over paid job board spend.

---

### Chapter 6: Is Leadership Fair?

Two questions about management were tested:

**Q1: Are managers scoring employees consistently, or is there bias?**

A Kruskal-Wallis H-test across all managers' performance score distributions returned **P-value = 0.975** — no statistically significant difference. Despite visible variation in individual rating styles (some managers lean strict, others generous), the differences are not significant at the population level.

> **Note:** The boxplot still shows some managers with notably tighter or wider scoring distributions — worth monitoring even if not yet statistically conclusive.

**Q2: Are any managers structurally overloaded?**

Cross-tabulating ManagerName against Department revealed managers simultaneously overseeing multiple departments — a structural risk for burnout, inconsistent oversight, and accountability gaps. These cases were flagged for organizational review.

---

### Chapter 7: The Boomer Attrition Puzzle

Generational analysis showed that **Boomers (>60) have a ~52% attrition rate** — nearly 20 percentage points above Millennials and Gen X. But here's the paradox: their engagement scores are just as high (median ~4.2/5).

This means **Boomers are leaving, but not because they're disengaged.** The driver is likely life-stage (retirement, health, caregiving) — not dissatisfaction. The typical "fix engagement to fix attrition" playbook won't work here.

Drilling deeper with a Department × Tenure heatmap:

| | Short-term (≤2 yrs) | Mid-term (3–5 yrs) | Long-term (>5 yrs) |
|---|---|---|---|
| **Production** | 3 | **5** ⚠️ | 3 |
| **IT/IS** | 0 | 2 | 0 |
| **Sales** | 0 | 1 | 0 |

**Production × Mid-term is the single highest-risk cell** — 5 exits from experienced employees leaving at peak productivity, not near retirement. This is operationally disruptive and largely preventable.

IT/IS and Sales are nearly clean — so this isn't a company-wide culture problem. It's a Production-specific issue, which makes it much more tractable to solve.

---

## 🛠️ Tools & Methods

| Category | Tools / Methods |
|---|---|
| Language | Python |
| Data Wrangling | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Statistical Testing | SciPy (Kruskal-Wallis H-test, Chi-Square Test) |
| Distribution Analysis | KDE (Kernel Density Estimation) |
| Segmentation | Job Tier mapping, Generational cohort bucketing, RFM-style anomaly scoring |

---

## 📁 Repository Structure

```
├── HR_Analytics.ipynb         # Full analysis notebook
├── HRDataset_v14.csv          # Source dataset (from Kaggle)
└── README.md
```

---

## 💡 Key Takeaways

1. **Missing data is not always random** — the Webster Butler ManagerID gap was structural, not accidental. Always investigate the pattern before imputing.
2. **Salary ≠ satisfaction, and satisfaction ≠ engagement** — these are three different levers that require different interventions.
3. **Statistical significance changes the decision** — the recruitment channel efficiency scores looked different across channels, but the Kruskal-Wallis test showed the differences weren't real. Acting on averages alone would have been misleading.
4. **Attrition patterns are structured, not random** — the bimodal resignation curve means early-tenure intervention has a clear, targeted window.
5. **Engagement paradox requires different thinking** — high engagement + high attrition (Boomers) means the problem isn't cultural. Trying to fix it with engagement programs would waste resources.

---

## 🚀 Possible Next Steps

- **Attrition prediction model** — use tenure, engagement, performance, and tier as features to predict which current employees are at highest resignation risk
- **Pay equity analysis** — test whether salary differences across gender or department are statistically significant after controlling for tier and performance
- **Succession planning dashboard** — build a Tableau view that maps long-tenure Boomers in Production against their department's key responsibilities

---

*Independent project using publicly available dataset from Kaggle.*  
*Statistical conclusions are based on available data and should be validated with larger or more recent datasets before operational use.*
