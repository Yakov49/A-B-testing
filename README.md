# A/B Test – CTA Copy on Checkout (Synthetic Case Study)

## Introduction
The test shows whether changing the cart CTA from **“Continue”** (A) to **“Continue to Secure Checkout”** (B) improves conversion.

- **Dataset:** 300 users (150 per variant), binary outcome `converted` (0/1)  
- **Primary metric:** Conversion rate (purchase completed)  
- **Unit of randomization:** user  
- **Test:** Two-proportion z-test, **two-sided**, α = 0.05

---

## Data

**File:** `ab_test_data.csv` (300 rows)

| column     | type | description                         |
|------------|------|-------------------------------------|
| user_id    | int  | anonymous user id                   |
| variant    | text | `A` (control) or `B` (test)         |
| converted  | int  | 0 = no purchase, 1 = purchase       |

---

## Results (matches the included data)

**Top-line**

| Variant | Conversions | Users | Conversion |
|---|---:|---:|---:|
| **A (control)** | 24 | 150 | **16.00%** |
| **B (test)**    | 41 | 150 | **27.33%** |

**Effect**
- Absolute lift: **+11.33 pp**
- Relative lift: **+70.8%** (27.33% vs 16.00%)

**Significance (two-sided)**
- z = **2.382**  
- p-value = **0.0172**  
- 95% CI for (p_B − p_A): **[+2.10 pp, +20.57 pp]**  
- **Decision:** **Ship Variant B** (statistically significant and practically meaningful)

> **External verification:** ABTestGuide (two-sided, 95%), A=24/150 vs B=41/150 → **p = 0.0162**, z = 2.4053, uplift **+70.83%**.  
> *(Insert your screenshot as `abtestguide.png` or replace with a link.)*

---

## Visuals

### Conversion rate with 95% CI
![Conversion rate with 95% CI](conversion_rates_ci.png)

### Effect size (B − A) with 95% CI
![Effect size CI](diff_ci.png)

---

## Reproduce Locally (Python)

```python
import pandas as pd, math
df = pd.read_csv("ab_test_data.csv")

# summary
summary = df.groupby("variant")["converted"].agg(['sum','count','mean'])
summary.columns = ["conversions","n","rate"]
print(summary)

# two-proportion z-test (pooled SE)
x1, n1 = summary.loc['A','conversions'], summary.loc['A','n']
x2, n2 = summary.loc['B','conversions'], summary.loc['B','n']
p1, p2 = x1/n1, x2/n2
p_pool = (x1+x2)/(n1+n2)
se = (p_pool*(1-p_pool)*(1/n1 + 1/n2))**0.5
z  = (p2 - p1) / se

from math import erf, sqrt
p_value = 2*(1 - (1+erf(abs(z)/sqrt(2)))/2)

# unpooled CI
se_un = (p1*(1-p1)/n1 + p2*(1-p2)/n2)**0.5
ci = ((p2-p1) - 1.96*se_un, (p2-p1) + 1.96*se_un)

print({"p_A":p1, "p_B":p2, "z":z, "p_value":p_value, "CI95":ci})
