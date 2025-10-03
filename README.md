# A/B Test – CTA Copy on Checkout (Synthetic Case Study)

## Introduction
The test shows whether changing the cart CTA from **“Continue”** (A) to **“Continue to Secure Checkout”** (B) improves conversion.

- **Dataset:** 300 users (150 per variant), binary outcome `converted` (0/1)  
- **Primary metric:** Conversion rate (purchase completed)  
- **Unit of randomization:** user  
- **Test:** Two-proportion z-test, **two-sided**, α = 0.05

---

## Data

**File:** [ab_test_data.csv](ab_test_data.csv) (300 rows)

| column     | type | description                         |
|------------|------|-------------------------------------|
| user_id    | int  | anonymous user id                   |
| variant    | text | `A` (control) or `B` (test)         |
| converted  | int  | 0 = no purchase, 1 = purchase       |

---

## Results

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

**External verification:** ABTestGuide (two-sided, 95%), A=24/150 vs B=41/150 → **p = 0.0162**, z = 2.4053, uplift **+70.83%**.  
![A/B test guide](/ab_images/ab_testguide.JPG)

---

## Visuals

### Conversion rate with 95% CI
![Conversion rate with 95% CI](/ab_images/conversion_rate.jpg)

### Effect size (B − A) with 95% CI
![Effect size CI](/ab_images/effect_size.jpg)

---

## Conclusion

Variant **B** (“Continue to Secure Checkout”) **outperformed** the control with a conversion rate of **27.33% vs 16.00%** — an absolute lift of **+11.33 percentage points** (**+70.8%** relative).  
The effect is **statistically significant** (two-sided z-test, **p = 0.0172**, 95% CI **[+2.10 pp, +20.57 pp]**).  
**Decision:** roll out **Variant B**.

**Interpretation**
- The stronger, reassurance-led CTA likely reduced hesitation at the cart step.
- The confidence interval suggests a **real, positive improvement** even at the lower bound (~+2 pp).

**Roll-out plan**
1. Deploy **B to 100%** of traffic.
2. **Monitor guardrails** for 2–4 weeks: overall conversion, revenue per visitor/AOV, refund rate, page speed, and support tickets.
3. Watch for **novelty effects** and confirm the lift sustains across devices and channels.

**Limitations**
- Synthetic dataset and single-metric focus (conversion only); no segmentation or seasonality controls here.
- No revenue or downstream retention analysis included.
